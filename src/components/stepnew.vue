<template>
  <div>
  <notifications group="bar" position="top right"></notifications>
  <notifications :duration="3000" group="foo" position="top right" >
      <template slot="body" slot-scope="props">
      <v-card color="#CCFF90">
        <v-card-text>
          <div>
            <span class="font-italic">{{props.item.data.behavior.name}} is </span>
            <span class="font-italic" v-if="props.item.data.result == 0">not</span>
            <span class="font-italic">approved</span>
          </div>
          <div class="font-italic">Comment: {{props.item.data.comment}}</div>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn v-if="(props.item.data.result == 0) && (props.item.data.img != 'teacher')" small @click="emit(props.item.data.img, props.item.data.behavior, props.item.data.name)">send to teacher again</v-btn>
        </v-card-actions>
      </v-card>
      </template>
    </notifications>
  <v-container>
    <v-layout>
      <v-flex xs3>
        <v-card flat tile>
          <v-card-title>
            <span class="font-italic title">
              <v-icon>touch_app</v-icon>Best Practice Suggestion
            </span>
          </v-card-title>
           <v-card-text>
            <v-btn @click="ppp = !ppp" v-popover:ccc.right v-if="currentBehaviors.length != 0" light :color="behaviorColor" class="tooltip-target font-italic">{{currentBehaviors[0].name}}</v-btn>
              <popover v-show="ppp" name="ccc" offset="16" v-if="currentBehaviors.length != 0">
                  <v-card min-height="400" min-width="400" tile hover elevation="13" :color="behaviorColor">
                    <v-card-text>
                      <div class="text-xs-center headline font-weight-bold font-italic">
                        Style Card
                      </div>
                      <v-card-media height="20"></v-card-media>
                        <span class="font-weight-medium subheading font-italic">
                          <code>Style Name: {{currentBehaviors[0].name}}</code>
                        </span>
                      <v-card-media height="20"></v-card-media>
                        <span class="font-weight-medium subheading font-italic">
                          <code>Validation Method: {{currentBehaviors[0].detectionMethod}}</code>
                        </span>
                      <v-card-media height="20"></v-card-media>
                        <span class="font-weight-medium subheading font-italic">
                          <code>Level: {{currentBehaviors[0].level}}</code>
                        </span>
                      <v-card-media height="20"></v-card-media>
                        <span class="font-weight-medium subheading font-italic">
                          <code>Style Description: {{currentBehaviors[0].description}}</code>
                        </span>
                      <v-card-media height="20"></v-card-media>
                      <v-layout>
                        <v-flex xs12>
                          <span v-if="currentBehaviors[0].goodExample" class="font-weight-medium subheading font-italic">
                            <code>Good Example of the style:</code>
                          </span>
                          <v-card-media height="10"></v-card-media>
                          <v-card-media contain height="200">
                            <v-img contain height="200" :src="currentBehaviors[0].goodExample"></v-img>
                          </v-card-media>
                        </v-flex>
                      </v-layout>
                      <v-card-media height="20"></v-card-media>
                      <v-layout>
                        <v-flex xs12>
                          <span v-if="currentBehaviors[0].badExample" class="font-weight-medium subheading font-italic">
                            <code>Bad Example of the style:</code>
                          </span>
                          <v-card-media height="10"></v-card-media>
                          <v-card-media contain height="200">
                            <v-img contain height="200" :src="currentBehaviors[0].badExample"></v-img>
                          </v-card-media>
                        </v-flex>
                      </v-layout>
                      <v-card-media height="30"></v-card-media>
                  </v-card-text>
                </v-card>
              </popover>
           </v-card-text>
        </v-card>
      </v-flex>


      <v-flex xs8 offset-xs1>
        <v-card>
          

          <!--        This is the teaching stuff                                   -->
          <v-card-title>
            <span class="display-1 font-weight-black font-italic blue-grey--text ma-2">Tutorial</span>
          </v-card-title>
          <v-card-media contain height="300">
            <v-img contain height="300" :src="tutorial"></v-img>
          </v-card-media>
          <v-card-media height="30"></v-card-media>
          <v-card-text>
            <div>
              <span class="headline font-weight-regular grey--text font-italic">{{currentSubsectionName}} Subsection</span>
            </div>
            <v-card-media height="30"></v-card-media>
            <div>
              <span class="title font-italic amber--text">Step Instruction</span>
            </div>
            <v-card-media height="30"></v-card-media>
            <div>
              <span class="subheading font-weight-light black--text">{{$store.state.project.step}}. {{currentStepContent}}</span>
            </div>
          </v-card-text>
           
          



              <v-card tile flat v-if="currentBehaviors.length != 0 && currentBehaviors[0].question">
                <v-card-text>
                  <span class="ma-1 font-italic">
                    Answer the following questions about the style!
                  </span>
                  <div>
                    {{currentBehaviors[0].question}}
                  </div>
                  <v-checkbox v-model="yourAnswer" v-for="(answer, index) in currentBehaviors[0].answerSets" :value="index" :key="answer.question" :label="answer.question"></v-checkbox>
                </v-card-text>
              </v-card>




          <v-card-actions v-if="currentBehaviors.length != 0">
            <v-spacer></v-spacer>
            <v-btn @click="submitAnswer" v-if="currentBehaviors[0].question && !answerQuestionCorrect" outline color="#E53935">Submit Answer</v-btn>
            <v-btn @click="nextStep" v-if="currentBehaviors[0].question && !arrSectionEnd && answerQuestionCorrect" outline color="#E53935">Next Step</v-btn>
            <v-btn @click="nextStep" v-if="currentBehaviors[0].question == '' && !arrSectionEnd" outline color="#E53935">Next Step</v-btn>
            <!--v-btn @click="nextStep" v-if="currentBehaviors[0].question && arrSectionEnd" :disabled="!fetchedBehavior" outline color="#E53935">Next Step</v-btn>
            <v-btn @click="nextStep" v-if="currentBehaviors[0].question == '' && arrSectionEnd" :disabled="!fetchedBehavior" outline color="#E53935">Next Step</v-btn-->
            <v-btn @click="nextStep" v-if="currentBehaviors[0].question && arrSectionEnd && answerQuestionCorrect" outline color="#E53935">Next Step</v-btn>
            <v-btn @click="nextStep" v-if="currentBehaviors[0].question == '' && arrSectionEnd" outline color="#E53935">Next Step</v-btn>
     
          </v-card-actions>
         
          <v-card-actions v-if="currentBehaviors.length == 0">
            <v-spacer></v-spacer>
            <v-btn @click="nextStep" v-if="!arrSectionEnd" outline color="#E53935">Next Step</v-btn>
            <v-btn @click="nextStep" v-if="arrSectionEnd" outline color="#E53935">Next Step</v-btn>
          </v-card-actions>
        
        </v-card>
      </v-flex>

    </v-layout>
    



    <modal name="right">
      <v-card flat tile>
        <v-card-media height="60"></v-card-media>
        <v-card-title class="font-italic font-weight-black green--text display-1">Right!</v-card-title>
      </v-card>
    </modal>
    <modal name="wrong">
      <v-card flat tile>
        <v-card-media height="60"></v-card-media>
        <v-card-title class="font-italic font-weight-black red--text display-1">Wrong!</v-card-title>
      </v-card>
    </modal>
    <modal name="over">
      <v-card flat tile>
        <v-card-media height="60"></v-card-media>
        <v-card-title class="font-italic font-weight-black blue--text display-2">Congratulations! Your project is done!</v-card-title>
      </v-card>
    </modal>

    <modal :clickToClose="false" name="sectionend">
      <v-card flat tile>
        <v-card-media height="60"></v-card-media>
        <v-card-title class="font-italic font-weight-black orange--text display-1">Please take a photo of your current breadboard using our app to continue!</v-card-title>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn @click="hide">Dismiss</v-btn>
        </v-card-actions>
      </v-card>
    </modal>

      <modal :clickToClose="false" :height="900" v-for="(review, index) in photoToReview" @closed="close(index)" :key="index" :name="index.toString()">
        <v-card flat tile>
          <v-card-title>
            <span class="font-weight-black font-italic title">
              Please review the {{review.behavior.name}} style
            </span>
            <span class="font-weight-bold headline amber--text" v-if="review.behavior.goodExample">
              The good example of the style is as follow:
            </span>
          </v-card-title>
          <v-card-media contain height="150" v-if="review.behavior.goodExample">
            <v-img contain :src="review.behavior.goodExample" height="150"></v-img>
          </v-card-media>
          <v-card-text>
            <span class="font-italic blue--text">
              This is the student's snapshot
            </span>
          </v-card-text>
          <v-card-media contain height="150">
            <v-img contain :src="review.img" height="150"></v-img>
          </v-card-media>
          <v-card-text>
            <v-text-field persistent-hint hint="Add some comment" background-color="#FCE4EC" v-model="photoToReview[index].comment"></v-text-field>
          </v-card-text>
          <v-card-actions>
            <v-layout>
              <v-flex xs10 offset-xs1>
                <v-select label="Select Comment" v-model="photoToReview[index].comment" :items="['Mind polarity', 'Wire is wrong']"></v-select>
              </v-flex>
            </v-layout>
          </v-card-actions>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn @click="sendReviewResult(index, 1, review.studentName, review.behavior, review.comment, review.img)">Right</v-btn>
            <v-btn @click="sendReviewResult(index, 0, review.studentName, review.behavior, review.comment, review.img)">Wrong</v-btn>
            <v-btn @click="closeDialog(index, review.img, review.studentName, review.behavior)">Not Sure</v-btn>
          </v-card-actions>
        </v-card>
      </modal>

      <modal name="pr" height="100">
        <v-card-media height="30"></v-card-media>
        <p class="text-xs-center">
          <v-icon x-large>thumb_up</v-icon>
        </p>
      </modal>
    
    

  </v-container>
  </div>
</template>

<script>
  export default {
    
    name: "Step",
    data: () => ({
      answerQuestionCorrect: false,
      yourAnswer: [],
      stepTot: 0,
      sectionBehaviors: [],
      fetchedBehavior: false,
      photoToReview: [],
      ppp: false
    }),
    methods: {
      hide: function() {
        this.$modal.hide("sectionend");
      },
      cc: function() {
        this.$modal.show("pr");
      },
      nextStep: function() {
        let sectionStep = this.$store.state.project.subsections[this.$store.state.project.currentSubsection - 1].steps;

        if ( this.$store.state.project.currentStepContent == sectionStep[sectionStep.length - 1] ) {
          this.$modal.show("sectionend");  
        }
        if (this.$store.state.project.step == this.stepTot)  {
          this.$modal.show("over");
          return;
        }
        this.fetchedBehavior = false;
        this.answerQuestionCorrect = false;
        this.yourAnswer = [];
        if (this.currentBehaviors.length != 0 && (this.currentBehaviors[0].question == '') && !this.sectionBehaviors.map((element) => (element.name)).includes(this.currentBehaviors[0].name)) {
          this.sectionBehaviors.push(this.currentBehaviors[0]);
        }
        this.$store.commit("project/addStep");
        this.$socket.emit("addStep", this.$store.state.student.studentName, this.$store.state.project.currentSubsection, this.$store.state.project.currentStepContent);
        
        sectionStep = this.$store.state.project.subsections[this.$store.state.project.currentSubsection - 1].steps;
        if ( this.$store.state.project.currentStepContent == sectionStep[sectionStep.length - 1] ) {
          if (this.currentBehaviors.length != 0 && (this.currentBehaviors[0].question == '') && !this.sectionBehaviors.map((element) => (element.name)).includes(this.currentBehaviors[0].name)) {
            this.sectionBehaviors.push(this.currentBehaviors[0]);
          }  
        }
        if (this.$store.state.project.currentStepContent == this.$store.state.project.subsections[this.$store.state.project.currentSubsection - 1].steps[0]) {
          for (let i = 0; i < this.photoToReview.length; ++i) {
            this.$modal.show(i.toString());    
          }
        }
      },
      close: function( index ) {
        if (index == 0) {
          this.photoToReview = [];
        }
      },
      
      submitAnswer: function() {
        let cmpAnswer = [];
        for (let answerSet = 0; answerSet < this.currentBehaviors[0].answerSets.length; ++answerSet) {
          if (this.currentBehaviors[0].answerSets[answerSet].check) {
            cmpAnswer.push(answerSet);
          }
        }
        if ( this.yourAnswer.toString() == cmpAnswer.toString() ) {
          this.$socket.emit("styleData", this.currentBehaviors[0].name, this.$store.state.student.studentName, 1);
          this.answerQuestionCorrect = true;
          this.$modal.show("right");
        } else {
          this.$socket.emit("styleData", this.currentBehaviors[0].name, this.$store.state.student.studentName, 0);
          this.answerQuestionCorrect = false;
          this.$modal.show("wrong");
        }
      },
      sendReviewResult: function(index, reviewResult, studentName, behavior, comment, img) {
        this.$modal.hide(index.toString());
        this.$socket.emit("reviewResult", reviewResult, studentName, behavior, comment, img);
      },
      closeDialog: function(index, img, studentName, behavior) {
        this.$modal.hide(index.toString());
        this.$socket.emit("photo", [img, studentName], behavior);
      },
      emit: function(img, behavior, name) {
        this.$socket.emit("review2Teacher", img, behavior, name);
      },
    },
    computed: {
      behaviorColor: function() {
        const mapping = {
          "High": "#E57373",
          "Medium": "#F9A825",
          "Low": "#A5D6A7"
        }
        return mapping[this.currentBehaviors[0].level];
      },
      currentStepNumber: function() {
        return this.$store.state.project.step;
      },
      tutorial: function() {
        return this.$store.state.project.settings["projectTutorial"];
      },
      currentSubsectionName: function() {
        return this.$store.state.project.subsections[this.$store.state.project.currentSubsection - 1].name;
      },
      currentStepContent: function() {
        return this.$store.state.project.currentStepContent;
      },
      currentBehaviors: function() {
          return this.$store.state.project.currentBehaviors;
      },
      arrSectionEnd: function() {
        let sectionStep = this.$store.state.project.subsections[this.$store.state.project.currentSubsection - 1].steps;
        return sectionStep[sectionStep.length - 1] == this.currentStepContent? true: false;
      }
      
    },
    created: function() {
      for (let subsection of this.$store.state.project.subsections)  {
        this.stepTot += subsection.steps.length;
      }
      if (this.currentBehaviors.length != 0 && this.currentBehaviors[0].question == '') {
        this.sectionBehaviors.push(this.currentBehaviors[0]);
      }
    },
    sockets: {
      sendFeedback: function(data) {
        this.$notify({
          group: "bar",
          title: "A feedback from teacher",
          text: data
        })
      },
      photo: function(data) {
        this.fetchedBehavior = true;
        this.$modal.hide("sectionend");
        console.log("original student received");
        console.log(data);
        if (this.sectionBehaviors.length != 0) {
          for (let behavior of this.sectionBehaviors) {
            console.log("emit to reviewing students");
            console.log(behavior.name);
            this.$socket.emit("photo", data, behavior);
          }
        }
        this.sectionBehaviors = [];
      },
      photoToJudge: function(data) {
        console.log("photo to judge");
        let img = data[0][0];
        let studentName = data[0][1];
        let behavior = data[1];

        this.photoToReview.push({
          img: img,
          studentName: studentName,
          behavior: behavior,
          comment: ""
        })
      },
      reviewResult: function( data ) {
        
        let reviewResult = data[0];
        let name = data[1];
        let behavior = data[2];
        let comment = data[3];
        let img = data[4];
        this.$socket.emit("styleData", {
          style: behavior.name,
          name: name,
          reviewResult: reviewResult
        });
        if (reviewResult == 1) {
          this.$notify({
            group: "foo",
            data: {
              behavior: behavior,
              result: reviewResult,
              comment: comment,
              img: img,
              name: name
            },
            type: "success",
            close: function() {
            }
          })
        }
        else {
          this.$notify({
            group: "foo",
            data: {
              behavior: behavior,
              result: reviewResult,
              comment: comment,
              img: img,
              name: name
            },
            type: "warn"
          })
        }
      },
      pr: function() {
        this.$modal.show("pr");
        let that = this;
        setTimeout ( function () {
          that.$modal.hide("pr");
        }, 3000);
      },
          
      
    
    }

  }
</script>

<style scoped>
.notifications {
  background-color: grey;
  border-left: 4px solid blue;
}

</style>

