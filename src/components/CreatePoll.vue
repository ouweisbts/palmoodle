
<template>
  

  <div class="container">
    <!-- Titre + description -->
    <h1>PollDLE</h1>
    <h2>Voting done simply in real-time</h2>

    

    <!-- PollDLE name -->
    <div class="row">
      <div class="col">
        <!-- Directive v-model with question -->
        <input
          v-model="question"
          type="text"
          class="large-input mx-auto d-block"
          placeholder="Add your question here"
        >
      </div>
    </div>

    <h3>Add your PollDLE options</h3>

    <div class="row">
      <div class="col">
        <!-- Directive v-model with newPolldleOptionText -->
        <!-- Directive v-on with addPolldleOption -->
        <input
          v-model="newPolldleOptionText"
          type="text"
          placeholder="Polldle Option"
          class="large-input mx-auto d-block"
          @keypress.enter="addPolldleOption"
        >
      </div>
    </div>
    <!-- Directive v-show with buttonShown -->
    <div
      v-show="buttonShown"
      class="row"
    >
      <div class="col">
        <!-- Directive v-on with clearAllPolldleOptions -->
        <button
          type="button"
          class="clear-button btn-lg btn-danger mx-auto d-block"
          @click="clearAllPolldleOptions"
        >
          Clear all PollDLE Options
        </button>
      </div>
    </div>

    <!-- PollDLE option -->
    <!-- Directive v-for with polldleOptions -->
    <div
      v-for="currentPolldleOption in polldleOptions"
      :key="currentPolldleOption.text"
      class="row justify-content-center"
    >
      <!-- Instance of CreatePolldleOption component -->
      <CreatePolldleOption />
    </div>

    <!-- Button Action -->
    <div class="row">
      <div class="col">
        <!-- Directive v-bind with isCreatePolldleDisabled() -->
        <!-- Directive v-on with createPolldle -->
        <button
          type="button"
          class="validate-button btn-lg btn-primary mx-auto d-block"
          :disabled="isCreatePolldleDisabled()"
          @click="createPolldle"
        >
          Create PollDLE
        </button>
      </div>
    </div>

    <div
      class="alert alert-primary"
      role="alert"
    >
      <h4 class="alert-heading">
        Summary of your PollDLE
      </h4>
      <hr>
      <p>
        The question is:
        <strong>
          <!-- Mustache with question -->
          <strong>{{ question }}</strong>
        </strong>
      </p>
      <!-- Mustache with computed property: listSize -->
      <p>Number of PollDLE options: {{ listSize }}</p>
    </div>

    <!-- Directive v-show with errorMessage -->
    <!-- Directive v-text with errorMessage -->
    <div
      v-show="errorMessage !== ''"
      class="error-message alert alert-danger"
      role="alert"
      v-text="errorMessage"
    />
  </div>
</template>

<script>
// Import CreatePolldleOption component
import CreatePolldleOption from "@/components/CreatePolldleOption.vue";

export default {
  name: "CreatePolldle",
  // Add dependencies on CreatePolldleOption component.
  components: { CreatePolldleOption },
  data() {
    return {
      question: "",
      newPolldleOptionText: "",
      polldleOptions: [],
      errorMessage: "",
      buttonShown: false
    };
  },
  // Watcher on polldleOptions
  watch: {
    polldleOptions() {
      this.buttonShown = this.polldleOptions != null && !(this.polldleOptions.length === 0);
    }
  },
  // Computed property listSize when polldleOptions changes
  computed: {
    listSize() {
      return this.polldleOptions.length;
    }
  },
  methods: {
    removedPolldleOption(polldleOption) {
      let index = this.polldleOptions.indexOf(polldleOption);
      this.polldleOptions.splice(index, 1);
      this.errorMessage = "";
    },

    addPolldleOption() {
      this.polldleOptions.push({
        text: this.newPolldleOptionText
      });
      this.newPolldleOptionText = "";
    },

    clearAllPolldleOptions() {
      this.polldleOptions = [];
      this.errorMessage = "";
    },

    createPolldle() {
      var polldleObject = {
        question: this.question,
        polldleOptions: []
      };

      this.polldleOptions.forEach(element => {
        var newPollOptionElement = { name: element.text };
        if (element.text !== "") {
          polldleObject.polldleOptions.push(newPollOptionElement);
        }
      });
    },

    isCreatePolldleDisabled() {
      return (
        this.polldleOptions === null ||
        this.polldleOptions.length < 2 ||
        this.question === ""
      );
    }
  }
};
</script>

<style>
@import url(https://fonts.googleapis.com/css?family=Roboto:300);

.login-page {
  width: 360px;
  padding: 8% 0 0;
  margin: auto;
}
.form {
  position: relative;
  z-index: 1;
  background: #FFFFFF;
  max-width: 360px;
  margin: 0 auto 100px;
  padding: 45px;
  text-align: center;
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.2), 0 5px 5px 0 rgba(0, 0, 0, 0.24);
}
.form input {
  font-family: "Roboto", sans-serif;
  outline: 0;
  background: #f2f2f2;
  width: 100%;
  border: 0;
  margin: 0 0 15px;
  padding: 15px;
  box-sizing: border-box;
  font-size: 14px;
}
.form button {
  font-family: "Roboto", sans-serif;
  text-transform: uppercase;
  outline: 0;
  background: #6d095f;
  width: 100%;
  border: 0;
  padding: 15px;
  color: #FFFFFF;
  font-size: 14px;
  -webkit-transition: all 0.3 ease;
  transition: all 0.3 ease;
  cursor: pointer;
}
.form button:hover,.form button:active,.form button:focus {
  background: #6d095f;
}
.form .message {
  margin: 15px 0 0;
  color: #b3b3b3;
  font-size: 12px;
}
.form .message a {
  color: #6d095f;
  text-decoration: none;
}
.form .register-form {
  display: none;
}
.container {
  position: relative;
  z-index: 1;
  max-width: 300px;
  margin: 0 auto;
}
.container:before, .container:after {
  content: "";
  display: block;
  clear: both;
}
.container .info {
  margin: 50px auto;
  text-align: center;
  color: #f2f2f2;
}
.container .info h1 {
  margin: 0 0 15px;
  padding: 0;
  font-size: 36px;
  font-weight: 300;
  color: #f2f2f2;
}
.container .info span {
  color: #4d4d4d;
  font-size: 12px;
}
.container .info span a {
  color: #f2f2f2;
  text-decoration: none;
}
.container .info span .fa {
  color: #EF3B3A;
}
body {
  background: #6d095f; /* fallback for old browsers */
  background: -webkit-linear-gradient(right, #6d095f, #6d095f);
  background: -moz-linear-gradient(right, #6d095f, #6d095f);
  background: -o-linear-gradient(right, #6d095f, #6d095f);
  background: linear-gradient(to left, #6d095f, #6d095f);
  font-family: "Roboto", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;      
}

	

</style>
