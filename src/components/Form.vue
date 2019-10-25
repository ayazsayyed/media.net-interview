<template>
  <div class="main">
    <div class="form-wrapper">
      <form class="form-inner" @submit.stop="checkForm($event)">
        <label for="emal">Email Address</label>
        <input
          type="text"
          name="email"
          class="add-user"
          autocomplete="off"
          v-model.trim="newEmailText"
        />
        <span class="error-msg" v-if="error.length>0">{{error}}</span>
        <input type="submit" value="Add" class="btn-add" />
      </form>
    </div>
    <div class="email-listings">
      <div class="search-bar-wrapper">
        <label for="search">Search</label>
        <input type="text" name="search" class="search-user" v-model="search" />
        <input type="submit" value="Go" class="btn-search" />
      </div>
      <div class="toggleEnable">
        <label for="toggleEnable">Show only Enabled</label>
        <input type="checkbox" name="toggleEnable" class="checkbox" @click="showOnlyEnabled" />
      </div>

      <ul class="email-list">
        <div v-if="!onlyCheckedEmails">
          <li class="list-item" v-for="(email,key) in filteredEmail" :key="key">
            <input
              type="checkbox"
              name="isEnabled"
              :checked="email.isChecked"
              @click="enableEmail(key)"
              class="checkbox"
            />
            <p class="emailId">{{email.email}}</p>
            <div class="action">
              <i class="fa fa-trash" aria-hidden="true" @click.stop="removeEmail(key)"></i>
            </div>
          </li>
        </div>

        <div v-if="onlyCheckedEmails">
          <li class="list-item" v-for="(email,key) in filterOnlyEnabled" :key="key">
            <input
              type="checkbox"
              name="isEnabled"
              :checked="email.isChecked"
              @click="enableEmail(key)"
              class="checkbox"
            />
            <p class="emailId">{{email.email}}</p>
            <div class="action">
              <i class="fa fa-trash" aria-hidden="true" @click.stop="removeEmail(key)"></i>
            </div>
          </li>
        </div>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "Form",
  data: function() {
    return {
      emails: [],
      error: "",
      newEmailText: null,
      search: "",
      onlyCheckedEmails: false
    };
  },
  computed: {
    filteredEmail() {
      return this.emails.filter(email => {
        return email.email.toLowerCase().includes(this.search.toLowerCase());
      });
    },
    filterOnlyEnabled() {
      return this.emails.filter(email => {
        return email.isChecked == true;
      });
    }
  },

  methods: {
    addnewEmail() {
      if (this.newEmailText.length > 0) {
        let newEmail = {
          isChecked: false,
          email: this.newEmailText
        };
        this.emails.push(newEmail);
        console.log(this.emails);
        this.newEmailText = "";
      }
    },
    removeEmail(key) {
      this.emails.splice(key, 1);
    },
    enableEmail(key) {
      this.emails[key].isChecked = !this.emails[key].isChecked;
    },
    showOnlyEnabled() {
      this.onlyCheckedEmails = !this.onlyCheckedEmails;
    },
    checkForm(e) {
      e.preventDefault();
      this.error = "";
      if (!this.newEmailText) {
        this.error = "Email required.";
      } else if (!this.validEmail(this.newEmailText)) {
        this.error = "Valid email required.";
      }
      if (!this.error.length) {
        console.log("form valid");
        this.addnewEmail();
        return true;
      }
    },
    validEmail(email) {
      var re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return re.test(email);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.main {
  display: flex;
}
.form-wrapper {
  width: 40%;
  padding: 15px;
}
.form-inner,
.search-bar-wrapper {
  display: flex;
  flex-direction: column;
}
input.add-user,
.search-user {
  margin: 10px 0 20px 0;
  height: 35px;
  font-size: 20px;
  padding: 0 11px;
}
.btn-add,
.btn-search {
  width: 150px;
  height: 35px;
  font-size: 18px;
  background: #43b6ec;
  color: #fff;
  border: none;
  border-radius: 5px;
  box-shadow: 0px 2px 8px #ccc;
}
.email-listings {
  width: 60%;
  padding: 15px;
}
.list-item {
  display: flex;
  align-items: center;
}
.email-list {
  padding: 0;
}
.action,
.checkbox {
  width: 10%;
}
.emailId {
  width: 80%;
}
.action i,
.checkbox {
  cursor: pointer;
}
.toggleEnable {
  display: flex;
  margin: 20px 0;
}
.form-inner {
  position: relative;
}
.error-msg {
  position: absolute;
  bottom: 30px;
  right: 0;
  color: red;
}
</style>
