<template>
  <v-card class="card">
    <div class="card-header">
      <v-img v-bind:src="student.avatar" class="card-image"></v-img>
      <div>
        <h1>{{ student.name }}</h1>
        <p>
          Ultima avaliação:
          {{ new Date(student.lastTest).toLocaleString("pt-BR").split(" ")[0] }}
        </p>
      </div>
    </div>
    <div class="card-content">
      <div>
        <p>{{ student.email }}</p>
        <p>
          {{ student.age }} {{ student.age == 1 ? "ano" : "anos" }}
          {{ student.cellphone }}
        </p>
      </div>
      <div class="buttons">
        <v-btn color="primary" fab small text elevation="2">
          <v-icon>mdi-pencil</v-icon>
        </v-btn>
        <v-btn color="primary" fab small text>
          <v-icon>mdi-dots-horizontal</v-icon>
        </v-btn>
      </div>
    </div>
  </v-card>
</template>

<script>
export default {
  name: "Card",
  props: {
    student: Object,
  },
};
</script>

<style scoped>
.card-header {
  position: relative;
  display: flex;
  align-items: center;
  width: 100%;
  border-bottom: 1px solid rgb(202, 202, 202);
  padding: 16px;
}

.card-header h1,
p {
  margin: 0;
    margin-bottom: 0 !important;

}

.card-header h1 {
  font-size: 1.5rem;
  line-height: 1;
}

.card-header p {
  font-size: 0.9rem;
}

.card-header .card-image {
  max-width: 40px;
  max-height: 40px;
  width: 40px;
  height: 40px;
  object-fit: cover;
  border-radius: 50%;
  margin-right: 8px;
}

.card-content {
  display: flex;
  padding: 16px;
  font-size: 0.8rem;
  color: rgb(128, 128, 128);
  align-items: center;
  justify-content: space-between;
  height: 80px;
}

.card-content p {
  max-width: 170px;
  padding-right: 8px;
}

.card-content .buttons {
  display: flex;
  align-items: center;
  justify-content: center;
}

@media (min-width:1920px) {
    .card-content p{
        max-width: 200px;
    }
}
</style>
