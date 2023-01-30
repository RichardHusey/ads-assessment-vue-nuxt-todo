<template>
  <v-container class="pa-4" fluid>
    <v-card>
      <v-card-title>
        TODO LIST
      </v-card-title>

      <v-card-text>
        <v-alert type="info">
          This page outlines requirements of the code challenge. Please complete at least 50+ points, and if you enjoy a
          challenge, we encourage you complete as much as you can.
        </v-alert>
        <v-list subheader two-line flat :todos.sync="todoList">
          <!-- TODO: Make this its own component -->
          <!-- <todo :title="domains" :items="[]"> -->

          <div v-for="(todo, i) in todoList" :key="i">
            <div>{{ todo.title }}</div>
            <div v-for="(todoItem, index) in todo.items" :key="index">
              <Todo :data="todoItem"></Todo>
            </div>
          </div>
        </v-list>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
import Todo from "../components/Todo.vue";

export default {
  components: {
    Todo
  },
  data() {
    return {
      checklist: [],
      todoList: [
        {
          title: "Dashboard",
          items: [
            {
              value: "active_domains",
              title: 'Domains',
              score: 4,
              subtitle: 'Retrieve total <strong>domains</strong> from endpoint <code> GET | /stats </code>.',
            },
            {
              value: "trashed_domains",
              title: 'Inactive Domains',
              score: 4,
              subtitle: 'Retrieve total <strong>inactive domains</strong> from endpoint <code>GET | /stats</code>.',
            },
            {
              value: "internationalized_domains",
              title: 'Active Domains',
              score: 4,
              subtitle: 'Retrieve total <strong>Active domains</strong> from endpoint <code>GET | /stats</code>.',
            },
            {
              value: "imprinted_domains",
              title: 'International Domains',
              score: 4,
              subtitle: 'Retrieve total <strong>international domains</strong> from endpoint <code>GET | /stats</code>.',
            },
          ]
        },

        {
          title: "Domains",
          items: [
            {
              value: "search_filter",
              title: 'Search Filter',
              score: 10,
              subtitle: 'Make the <strong>search input field</strong> filter the domains displayed by asynchronously querying api endpoint <code>GET | /domains?domain_name={domain}"</code>.',
            },
            {
              value: "delete_domains",
              title: 'Delete Domains',
              score: 10,
              subtitle: 'Make the <strong>delete icon</strong> remove domain using api endpoint <code> DELETE | /domains/{id}</code>.',
            },
            {
              value: "toggle_imprint",
              title: 'Toggle Imprint',
              score: 10,
              subtitle: 'Make the <strong>imprint icon</strong> toggle the imprint flag for a domain by changing icon color when true/false. <code> PUT | /domains/{id}</code>.',
            },
            {
              value: "load_component",
              title: 'Use <code>AddDomain</code> Component',
              score: 20,
              subtitle: `Load the <strong>Add Domain</strong> component into the <code><\ v- dialog ></code> shown when clicking plus icon at bottom of Domain page.`,
            },
            {
              value: "add_domains",
              title: 'Add Domains',
              score: 20,
              subtitle: '<p>Make the <strong>add domains form</strong> store new domains. <code> POST | /domains</code>.</p>',
            },
          ]
        },
        {
          title: "Code Challenge",
          items: [
            {
              value: "todo_list_into_component",
              title: 'TODO LIST',
              score: 20,
              subtitle: 'Convert the template into a dynamic property driven component. See file <code>/pages/code-challenge.vue</code> for in-code comments.',
            }
          ]
        },
        {
          title: "Bonus",
          items: [
            {
              value: "dark_mode",
              title: 'Dark Mode',
              score: 4,
              subtitle: 'Make theme dark/light mode selection persist through page refresh and navigation.',
            }
          ]
        }
      ]
    }
  },
  mounted() {
    this.checklist = JSON.parse(localStorage.getItem('checklist')) || [];
  },
  watch: {
    checklist: function (checklist) {
      localStorage.setItem('checklist', JSON.stringify(checklist));
    }
  }
}
</script>
