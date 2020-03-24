<template>
  <div>
    <b-form @submit="onSubmit" @reset="onReset">
      <b-form-group id="input-group-2" label="Your Name:" label-for="input-2">
        <b-form-input id="input-2" v-model="user.name" required placeholder="Enter name"></b-form-input>
      </b-form-group>

      <b-form-group
        id="input-group-1"
        label="Email address:"
        label-for="input-1"
        description="We'll never share your email with anyone else."
      >
        <b-form-input
          id="input-1"
          v-model="user.email"
          type="email"
          required
          placeholder="Enter email"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-3" label="Enter Password:" label-for="input-3">
        <b-form-input
          id="input-3"
          type="password"
          v-model="user.password"
          required
          placeholder="Enter password"
        ></b-form-input>
      </b-form-group>

      <b-button class="buttonClass" type="submit" variant="primary">Submit</b-button>
      <b-button type="reset" variant="danger">Reset</b-button>

      <b-form-group label="Generate Password Options:">
        <b-form-checkbox-group id="checkbox-group-1" v-model="selected" name="password-option">
          <b-form-checkbox :value="characters[0].name">
            {{
            characters[0].name
            }}
          </b-form-checkbox>
          <b-form-checkbox :value="characters[1].name">
            {{
            characters[1].name
            }}
          </b-form-checkbox>
          <b-form-checkbox :value="characters[2].name">
            {{
            characters[2].name
            }}
          </b-form-checkbox>
          <b-form-checkbox :value="characters[3].name">
            {{
            characters[3].name
            }}
          </b-form-checkbox>
        </b-form-checkbox-group>
      </b-form-group>

      <b-button @click="generatePassword" variant="info">Generate Password</b-button>
    </b-form>
  </div>
</template>

<script>
import db from "../fb";

export default {
  data() {
    return {
      users: [],
      user: {
        user_id: "",
        name: "",
        email: "",
        password: ""
      },
      gLength: 16,
      selected: ["Lowercase", "Uppercase"],
      characters: [
        {
          name: "Lowercase",
          value: "abcdefghijklmnopqrstuvwxyz",
          checked: false
        },
        {
          name: "Uppercase",
          value: "ABCDEFGHIJKLMNOPQRSTUVWXYZ",
          checked: false
        },
        {
          name: "Numbers",
          value: "0123456789",
          checked: false
        },
        {
          name: "Special Characters",
          value: "_-+=)(*&^%$#@!`~",
          checked: false
        }
      ]
    };
  },
  methods: {
    onSubmit() {
      this.users.push(this.user);
      db.collection("users").add(this.user);
    },
    onReset() {
      this.user = {
        name: "",
        email: "",
        password: ""
      };
      this.selected = ["Lowercase", "Uppercase"];
    },
    generatePassword() {
      let result = "";
      let charactersVal = "";

      //check which option is checked
      for (let z = 0; z < this.characters.length; z++) {
        if (this.selected[z] === this.characters[z].name) {
          this.characters[z].checked = true;
        }
      }
      //add the character value if an option is checked
      for (let j = 0; j < this.characters.length; j++) {
        if (this.characters[j].checked) {
          charactersVal += this.characters[j].value;
        }
      }
      //generate password
      for (let i = 0; i < this.gLength; i++) {
        result += charactersVal.charAt(
          Math.floor(Math.random() * charactersVal.length)
        );
      }
      //store result into user password
      this.user.password = result;
    }
  }
};
</script>

<style>
.buttonClass {
  margin-right: 5px;
}
</style>
