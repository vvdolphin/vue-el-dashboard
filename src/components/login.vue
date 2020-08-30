<template>

  <div id="table">
    <el-button type="primary" size="small" @click="addFormVisible = true">Add Course</el-button>
    <el-table :data="courses" border style="width: 100% ">
      <el-table-column prop="id" label="CourseId"></el-table-column>
      <el-table-column prop="name" label="CourseName"></el-table-column>
      <el-table-column prop="teacherId" label="teacherId"></el-table-column>
      <el-table-column label="Operator">
        <template slot-scope="scope">
          <el-button type="primary" size="small" plain @click="editCourse(scope.$index,courses)">Edit</el-button>
          <el-button type="primary" size="small" @click="deleteCourse(scope.$index,courses)">Delete</el-button>
        </template>
      </el-table-column>
    </el-table>

    <el-dialog title="Update the Course" :visible="editCourseFormVisible" size="tiny">
      <el-form :model="editForm" ref="editForm" label-width="120px">
        <el-form-item label="CourseId" prop="id">
          <el-input readonly="true" v-model="editForm.id"/>
        </el-form-item>
        <el-form-item label="name" prop="name">
          <el-input v-model="editForm.name"/>
        </el-form-item>
        <el-form-item label="teacher" prop="teacherId">
          <el-input v-model="editForm.teacherId"/>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitEditCourse()">Add</el-button>
          <el-button type="primary" @click="editCourseFormVisible =false">Cancle</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

    <el-dialog title="Add" :visible="addFormVisible">
      <el-form :model="addForm" ref="addForm">
        <el-form-item label="CourseId">
          <el-input v-model="addForm.id"/>
        </el-form-item>
        <el-form-item label="CourseName">
          <el-input v-model="addForm.name"/>
        </el-form-item>
        <el-form-item label="Teacher">
          <el-input v-model="addForm.teacherId"/>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitAddCourse()">Add</el-button>
          <el-button type="primary" @click="addFormVisible =false">Cancle</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    name: 'login',
    data() {
      return {
        editCourseFormVisible: false,
        addFormVisible: false,
        editForm: [],
        addForm: [],
        courses: []
      }
    },

    mounted() {
      this.$axios.get().then(response => {
        this.courses = response.data;
      }).catch(error => {
        alert(error)
      });
    },


    methods: {
      editCourse(index, courses) {
        this.editCourseFormVisible = true;
        this.editForm = courses[index];
      },
      deleteCourse(index, courses) {
        this.$confirm('此操作将永久删除该数据, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.courses.splice(index, 1);
          var id = courses[index].id - 1;
          this.$axios.put('/' + id).then(response => {
            this.$message({
              type: 'success',
              message: '删除成功!',
            })
          }).catch((err) => {
            this.$message({
              type: 'error',
              message: err
            })
          })
        })
      },

      submitEditCourse() {
        var id = this.editForm.id;
        this.$axios.put('/update/'+id,{
          name:this.editForm.name,
          teacherId:this.editForm.teacherId
        }).then(response => {
          this.$message({
            type: 'success',
            message: '修改成功!',
          })
        }).catch(error => {
          this.$message({
            type: 'error',
            message: error
          })
        });
        this.editCourseFormVisible =false
      },

      submitAddCourse() {
        this.$axios.post('',{
          id:this.addForm.id,
          name:this.addForm.name,
          teacherId:this.addForm.teacherId
        }).then(response => {
          this.courses = this.courses || [];
          this.courses.push({
            id: this.addForm.id,
            name: this.addForm.name,
            teacherId: this.addForm.teacherId,
          });
            this.$message({
            type: 'success',
            message: '修改成功!',
          })
        }).catch(error => {
          this.$message({
            type: 'error',
            message: error
          })
        });
        this.addFormVisible =false
      }

    }
  }
</script>


