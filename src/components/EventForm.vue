<template>
  <div id="newEvent">
    <b-form @submit.prevent="onAdd">
      <!-- min in match -->
      <b-form-group
        id="input-group-minInMatch"
        label-cols-sm="3"
        label="Minute In Match:"
        label-for="minInMatch"
      >
        <b-form-input
          id="minInMatch"
          v-model="$v.form.minInMatch.$model"
          type="text"
          :state="validateState('minInMatch')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.minInMatch.required">
          minute in match is required
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-else-if="!$v.form.minInMatch.between">
          minute in match should be between 0-90
        </b-form-invalid-feedback>
      </b-form-group>

      <!-- description -->
      <b-form-group
        id="input-group-description"
        label-cols-sm="3"
        label="Description:"
        label-for="description"
      >
        <b-form-input
          id="description"
          v-model="$v.form.description.$model"
          type="text"
          :state="validateState('description')"
        ></b-form-input>
        <b-form-invalid-feedback>
          description is required
        </b-form-invalid-feedback>
      </b-form-group>

      <!-- teamName -->
      <b-form-group
        id="input-group-teamName"
        label-cols-sm="3"
        label="Team Name:"
        label-for="teamName"
      >
        <b-form-select
          id="teamName"
          v-model="$v.form.teamName.$model"
          :options="teamNames"
          :state="validateState('teamName')"
        ></b-form-select>
        <b-form-invalid-feedback>
          Team Name is required
        </b-form-invalid-feedback>
      </b-form-group>

      <!-- type -->
      <b-form-group
        id="input-group-type"
        label-cols-sm="3"
        label="Type:"
        label-for="type"
      >
        <b-form-select
          id="type"
          v-model="$v.form.type.$model"
          :options="types"
          :state="validateState('type')"
        ></b-form-select>
        <b-form-invalid-feedback> Type is required </b-form-invalid-feedback>
      </b-form-group>

      <!-- playerName -->
      <b-form-group
        id="input-group-playerName"
        label-cols-sm="3"
        label="Player Name:"
        label-for="playerName"
      >
        <b-form-input
          id="playerName"
          v-model="$v.form.playerName.$model"
          type="text"
          :state="validateState('playerName')"
        ></b-form-input>
        <b-form-invalid-feedback>
          Player Name is required
        </b-form-invalid-feedback>
      </b-form-group>
      <b-button type="submit">Submit</b-button>
    </b-form>
    <b-alert
      class="mt-2"
      v-if="form.submitError"
      variant="warning"
      dismissible
      show
    >
      Add failed: {{ form.submitError }}
    </b-alert>
  </div>
</template>

<script>
import { required, between } from "vuelidate/lib/validators";
export default {
  name: "EventForm",
  data() {
    return {
      types: [
        "Goal",
        "Offside",
        "Fault",
        "Red Card",
        "Yellow Card",
        "Injury",
        "Substitution",
      ],
      teamNames: [],
      form: {
        minInMatch: "",
        description: "",
        teamName: "",
        type: "",
        playerName: "",
        submitError: undefined,
      },
    };
  },
  props: {
    matchData: {
      type: Object,
      required: true,
    },
  },
  validations: {
    form: {
      minInMatch: {
        required,
        between: between(0, 90),
      },
      description: {
        required,
      },
      teamName: {
        required,
      },
      type: {
        required,
      },
      playerName: {
        required,
      },
    },
  },
  mounted() {
    console.log("event form mounted");

    this.teamNames.push(this.matchData.homeTeam);
    this.teamNames.push(this.matchData.awayTeam);
    // console.log($v);
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },

    onAdd() {
      this.form.submitError = undefined;
      this.$v.form.$touch();
      if (this.$v.form.$anyError) {
        return;
      }
      this.$emit("closeModalEvent", {
        minInMatch: this.form.minInMatch,
        description: this.form.description,
        teamName: this.form.teamName,
        type: this.form.type,
        playerName: this.form.playerName,
      });
    },
    // handleChange() {
    //   console.log("handling data change");
    //   this.$emit("dataUpdated", );
    //   console.log("dataUpdated event fired");
    // },
  },
};
</script>

<style>
</style>