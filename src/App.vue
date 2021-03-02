<template>
  <v-app id="inspire">
    <header>
      <div class="header-content">
        <div class="header-shadow">
          <v-img src="https://github.com/matheusm.png"></v-img>
          <div class="header-info">
            <h1>Jonas DOe</h1>
            <p>Ultimo login: 12/12/1212</p>
          </div>
          <v-icon  style="color:white;">
            mdi-arrow-down
          </v-icon>
        </div>
      </div>
    </header>

    <v-main>
      <v-container>
        <v-row class="justify-space-between">
          <div class="col-md-5 col-lg-4">
            <v-text-field
              solo
              dense
              label="Busque por nomes ou emails"
              v-model="search"
              append-icon="mdi-magnify"
            ></v-text-field>
          </div>
          <v-dialog v-model="dialog" persistent max-width="600px">
            <template v-slot:activator="{ on, attrs }">
              <div
                class="col-md-2 d-flex justif-center justify-md-end justify-lg-end"
              >
                <v-btn dense color="red darken-1" dark v-bind="attrs" v-on="on">
                  <v-icon left>
                    mdi-account-plus
                  </v-icon>
                  Novo Aluno
                </v-btn>
              </div>
            </template>
            <v-card>
              <v-toolbar color="primary darken-3" dark>Castrar Aluno</v-toolbar>
              <v-card-text class="pb-0">
                <v-form>
                  <v-container>
                    <v-text-field
                      label="Nome"
                      type="text"
                      v-model="studentName"
                      filled
                    ></v-text-field>
                    <v-text-field
                      label="Email"
                      type="email"
                      v-model="studentEmail"
                      required
                      filled
                    ></v-text-field>
                    <v-text-field
                      type="number"
                      label="Idade"
                      v-model="studentAge"
                      required
                      filled
                    ></v-text-field>
                    <v-text-field
                      type="tel"
                      label="Telefone"
                      filled
                      v-model="studentCellphone"
                      required
                    ></v-text-field>
                    <v-text-field
                      type="url"
                      label="Url do Avatar"
                      filled
                      v-model="studentAvatar"
                    ></v-text-field>
                  </v-container>
                </v-form>
              </v-card-text>
              <v-card-actions class="justify-space-between pr-8 pl-8 pb-4 pt-0">
                <v-btn color="blue darken-1" text @click="cancelForm">
                  Cancelar
                </v-btn>
                <v-btn color="red" width="150" dark @click="addStudent">
                  Salvar
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-row>

        <v-row class="d-flex ">
          <div class="col-12 d-flex justify-center">
            {{ students.length === 0 ? "Não há usuários cadastrados" : "" }}
          </div>
          <div
            v-for="(student, n) in filteredList"
            v-bind:key="n"
            class="col-sm-6 col-md-3"
          >
            <Card v-bind:student="student" />
          </div>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Card from "./components/Card";

export default {
  name: "App",

  components: {
    Card,
  },

  data: () => ({
    search:'',
    students: [],
    newStudent: null,
    dialog: false,
    studentName: null,
    studentEmail: null,
    studentAge: null,
    studentCellphone: null,
    studentAvatar: null,
  }),

  mounted() {
    if (localStorage.getItem("students")) {
      try {
        this.students = JSON.parse(localStorage.getItem("students"));
      } catch (e) {
        localStorage.removeItem("students");
      }
    }
  },

  methods: {
    addStudent() {
      if (
        !this.studentName ||
        !this.studentEmail ||
        !this.studentAge ||
        !this.studentCellphone ||
        !this.studentAvatar
      ) {
        return;
      }
      this.dialog = false;
      this.students.push({
        id: Date.now(),
        name: this.studentName,
        email: this.studentEmail,
        age: this.studentAge,
        cellphone: this.studentCellphone,
        lastTest: Date.now(),
        avatar: this.studentAvatar,
      });
      this.clearForm();
      this.saveStudent();
    },
    removeStudent(x) {
      this.students.splice(x, 1);
      this.saveStudent();
    },
    saveStudent() {
      const parsed = JSON.stringify(this.students);
      localStorage.setItem("students", parsed);
    },

    clearForm() {
      this.studentName = null;
      this.studentEmail = null;
      this.studentAge = null;
      this.studentCellphone = null;
      this.studentAvatar = null;
    },

    cancelForm() {
      // this.clearForm();
      this.dialog = false;
    },
  },
  computed: {
    filteredList() {
      return this.students.filter(student => {
        return student.name.toLowerCase().includes(this.search.toLowerCase())
      })
    }
  },

  watch: {
    students(newStudents) {
      localStorage.students = newStudents;
    },
  },
};
</script>

<style>
p{
  margin: 0;
}
header {
  width: 100%;
  height: 80px;
  background-color: hsl(0, 100%, 50%);
  display: flex;
  align-items: center;
  justify-content: center;
}

header .header-content {
  width: 100%;
  height: 100%;
  max-width: 1150px;
  display: flex;
  justify-content: flex-end;
}

header .header-content > .header-shadow {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 300px;
  background-color: red;
  box-shadow: 0px 0px 17px 4px rgba(0, 0, 0, 0.1);
}
header .header-content .header-shadow .header-info{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  height: 100%;
  border-right: 1px solid white;
  padding-right: 16px;
}

header .header-content .header-shadow h1{
  font-size: 1rem;
  color: white;
  line-height: 1;
}

header .header-content .header-shadow p{
  color: white;
  font-size: 0.8rem;
  margin: 0;
}

header .header-content .header-shadow i{
  margin:0 16px;
}

header .header-content .header-shadow .v-image {
  max-width: 32px;
  max-height: 32px;
  widows: 100%;
  height: 100%;
  margin-right: 8px;
  border-radius: 50%;
  object-fit: cover;
}
</style>
