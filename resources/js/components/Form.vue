<template>
  <div class="modal-card">
        <header class="modal-card-head">
            <p class="modal-card-title">{{ isType }}</p>
        </header>
        <section class="modal-card-body">
          <b-field label="Name">
                <b-input
                    type="text"
                    v-model="name"
                    placeholder="Enter Name"
                    required>
                </b-input>
            </b-field>
            <b-field label="Email">
                <b-input
                    type="email"
                    v-model="email"
                    placeholder="Your email"
                    required>
                </b-input>
            </b-field>
            <div class="field">
              <label class="label">Select a birthdate</label>
                <div class="control is-clearfix">
                  <input v-model="birthdate" type="date" placeholder="Your email"class="input" required>
                </div>
            </div>

            <b-field label="Gender"></b-field>
              <b-radio v-model="gender"
                  name="gender"
                  native-value="Male">
                  Male
              </b-radio>
              <b-radio v-model="gender"
                  name="gender"
                  native-value="Female">
                  Female
              </b-radio>
            
        </section>
        <footer class="modal-card-foot">
            <button class="button" type="button" @click="$parent.close()">Close</button>
            <button class="button is-primary" v-if="isType=='Edit'" @click="updateSubmit">Submit</button>
            <button class="button is-primary" v-else @click="createSubmit">Submit</button>
        </footer>
    </div>
</template>
<script>
export default {
  name: 'FormModal',
  // props: ['isType', 'formData'],
  props: {
    isType: {
      type: String,
      default: 'Create'
    },
    formData: {
      name: {
        type: String,
        default: null,
      },
      email: {
        type: String,
        default: null,
      },
      birthdate: {
        type: String,
        default: null,
      },
      gender: {
        type: String,
        default: null,
      }
    }
  },
  data () {
    return {
      name: this.formData.name,
      email: this.formData.email,
      birthdate: this.formData.birthdate,
      gender: this.formData.gender,
    }
  },
  methods: {
    updateSubmit() {
      const data = {
        id: this.formData.id,
        name: this.name,
        email: this.email,
        birthdate: this.birthdate,
        gender: this.gender,
      }
      this.$emit('updateData', data);
    },
    createSubmit() {
      const data = {
        name: this.name,
        email: this.email,
        birthdate: this.birthdate,
        gender: this.gender,
      }
      if(this.validCheck(data)) {
        this.$emit('createData', data);
      }
    },
    validCheck(data) {
      for (var key in data) {
        if (data[key] == null || data[key] == "" || typeof data[key] == undefined) {
          this.$buefy.toast.open({
              message: `Please fill all details`,
              type: 'is-danger'
          });
          return false;
        }
      }
      return true;      
    }
  }
}
</script>