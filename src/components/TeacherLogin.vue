<template>
  <v-container>
    <v-layout>
      <v-flex xs8 offset-xs2>
        <v-card>
          <v-card-media height="200">
          </v-card-media>
          <v-card-actions>
            <v-btn @click="jumpToAuthoring" dark round ripple>
              <v-icon>arrow_left</v-icon>
              Authoring Tool
            </v-btn>
            <v-spacer></v-spacer>
            <v-btn dark @click="jump2Teacher" round ripple>
              Classroom Management
              <v-icon>arrow_right</v-icon>
            </v-btn>
          </v-card-actions>
          <v-card-media height="200"></v-card-media>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
  export default {
    name: "teacherLogin",
    methods: {
      jumpToAuthoring: function() {
        this.$store.commit("project/init");
        this.$router.push("/authoring");
      },
      jump2Teacher: function() {
        this.$socket.emit("teacherLogin");
        this.$router.push("/teacher");
      }
    },

  }
</script>