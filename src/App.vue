<template>
  <div id="app">
    <main class="content grid" id="home-content">
      <div class="content-title-container">
        <h2>Accounts</h2>
      </div>
      <section class="grid" id="account-grid">
        <section class="account-column grid" id="active-account-column">
          <div class="account-container-title" id="active-account-container-title">
            <h3>Active</h3>
          </div>
          <div class="account-container active-account">
            <ul class="account-data-list" v-for="account in getActiveAccounts" v-bind:key="account.id">
              <li><label>Name:</label>{{ account.LastName }}, {{ account.FirstName }}</li>
              <li><label>Email:</label>{{ account.Email }}</li>
              <li><label>Phone Number:</label>{{ account.PhoneNumber | formatPhone }}</li>
              <li><label>Amount Due:</label>{{ account.AmountDue | formatAmountDue }}</li>
              <li v-if="account.PaymentDueDate != null"><label>Due Date:</label>{{ account.PaymentDueDate | formatDateDue }}</li>
              <br>
            </ul>
          </div>     
        </section>
        <section class="account-column grid" id="overdue-account-column">
          <div class="account-container-title" id="overdue-account-container-title">
            <h3>Overdue</h3>
          </div>
          <div class="account-container overdue-account">
            <ul class="account-data-list" v-for="account in getOverdueAccounts" v-bind:key="account.id">
              <li><label>Name:</label>{{ account.LastName }}, {{ account.FirstName }}</li>
              <li><label>Email:</label>{{ account.Email }}</li>
              <li><label>Phone Number:</label>{{ account.PhoneNumber | formatPhone }}</li>
              <li><label>Amount Due:</label>{{ account.AmountDue | formatAmountDue }}</li>
              <li v-if="account.PaymentDueDate != null"><label>Due Date:</label>{{ account.PaymentDueDate | formatDateDue }}</li>
              <br>
            </ul>
          </div>
        </section>
        <div class="account-column grid" id="inactive-account-column">
          <div class="account-container-title" id="inactive-account-container-title">
            <h3>Inactive</h3>
          </div>
          <div class="account-container inactive-account">
            <ul class="account-data-list" v-for="account in getInactiveAccounts" v-bind:key="account.id">
              <li><label>Name:</label>{{ account.LastName }}, {{ account.FirstName }}</li>
              <li><label>Email:</label>{{ account.Email }}</li>
              <li><label>Phone Number:</label>{{ account.PhoneNumber | formatPhone }}</li>
              <li><label>Amount Due:</label>{{ account.AmountDue | formatAmountDue }}</li>
              <li v-if="account.PaymentDueDate != null"><label>Due Date:</label>{{ account.PaymentDueDate | formatDateDue }}</li>
              <br>
            </ul>
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'app',
  data () {
    return {
      accounts: []
    }
  },
  computed: {
    getActiveAccounts: function() {
      return this.getAccounts(0);
    },
    getOverdueAccounts: function() {
      return this.getAccounts(2);
    },
    getInactiveAccounts: function() {
      return this.getAccounts(1);
    }
  },
  filters: {
    formatPhone: function (value) {
      return "(" + value.substring(0, 3) + ")-"
         + value.substring(3, 6) + "-"
         + value.substring(6, 10);
    },
    formatAmountDue: function (value) {
      return "$" + parseInt(value) / 10 < 1 ? "0" + value.toFixed(2) : value.toFixed(2) + " USD";
    },
    formatDateDue: function (value) {
      var splitDate = value.substring(0, 10).split("-")
      return "(" + splitDate[1] + "/" + splitDate[2] + "/" + splitDate[0] + ")";
    }
  },
  methods: {
    getAccounts: function(status) {
      var result = []
      this.accounts.forEach(element => {
        if (element.AccountStatusId === status) {
          result.push(element);
        }
      });
      return result;
    }
  },
  created () {
    axios.get('https://frontiercodingtests.azurewebsites.net/api/accounts/getall').then(response => (this.accounts = response.data))
  }
}
</script>

<style lang="scss">
html, body {
  height: 100%;
  width: 100%;
  font-family: Roboto, Helvetica, Arial, sans-serif
}
article {
  height: 100%;
  grid-template-columns:1fr;
  grid-template-areas:
      "header"
      "main"
      "footer";
  grid-template-rows: 100px 1fr 150px;
}
header {
  grid-area: header;
  background-color:#006643;
  color: #fff;
  grid-template-columns: 1% 98% 1%;
}
h1 {
  font-size: 3em;
  font-weight: bold;
}
h2 {
  font-size: 2em;
  color:#006643;
}
h3 {
  display: block;
  font-size: 1.5em;
  color:#006643;
}
main {
  grid-area: main;
}
footer {
  grid-area: footer;
  background-color:#006643;
  color: #fff;
  grid-template-columns: 1% 98% 1%;
  grid-template-rows: 5% 90% 5%;
}
.grid {
  display: grid;
}
.title-container {
  grid-column-start: 2;
  align-self: center;
}
#home-content {
  grid-row-gap: 25px;
  grid-template-rows: 10px 100px auto;
}
#account-grid {
  grid-template-columns: 1fr 1fr 1fr;
  grid-row-start: 3;
}
.account-column {
  grid-template-rows: 2em repeat(2, 10em);
}
.content-title-container {
  justify-self: center;
  height: 1em;
  grid-row-start: 2;
}
#overdue-account-container-title > h3{
  color: #B22222;
}
#inactive-account-container-title > h3 {
  color:#808080;
}
.account-container {
  justify-self: center;
  height: 5em;
}
.account-container-title {
  justify-self: center;
}
.account-data-list {
  list-style: none;
}
.account-data-list > li {
  margin: 10px 0;
}
.account-data-list > li > label {
  font-weight: bold;
  margin: 0 5px 0 0;
}
</style>
