<template>
  <v-container>
    <v-layout>
      <v-flex xs8 offset-xs2>
        <v-card>
          <v-card-media height="400">
            <v-layout>
              <v-flex xs6>
                <v-img @click="loginAsAStudent" class="hover-hand" src="static/student.png"></v-img>
              </v-flex>
              <v-flex xs6>
                <v-img @click="loginAsATeacher" class="hover-hand" src="static/teacher.png"></v-img>
              </v-flex>
            </v-layout>
          </v-card-media>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
  export default {
    data: () => ({
    }),
    methods:{
      loginAsAStudent: function() {
        this.$router.push("/inputStudentName");
      },
      loginAsATeacher: function() {
        this.$router.push("/teacherLogin");
      },
    }
  } 
</script>

<style>
  .hover-hand {
    cursor: pointer
  }
</style>
