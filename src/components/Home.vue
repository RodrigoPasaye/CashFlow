<template>
  <Layout>
    <template #header>
      <Header></Header>
    </template>

    <template #resume>
      <Resume
        :totalLabel="'Ahorro Total'"
        :label="label"
        :totalAmount="totalAmount"
        :amount="amount"
      >
        <template #graphic>
          <Graphic :amounts="amounts" @select="select" />
        </template>

        <template #action><Action @create="create" /></template>
      </Resume>
    </template>

    <template #movements>
      <Movements :movements="movements" @remove="remove"></Movements>
    </template>
  </Layout>
</template>

<script>
import Layout from "@/components/Layout.vue";
import Header from "@/components/Header.vue";
import Resume from "@/components/resume/Index.vue";
import Graphic from "@/components/resume/Graphic.vue";
import Movements from "@/components/movements/Movements.vue";
import Action from "@/components/Action.vue";

export default {
  components: {
    Layout,
    Header,
    Resume,
    Movements,
    Action,
    Graphic,
  },
  data: () => ({
    amount: null,
    label: null,
    movements: [
      // {
      //   id: 1,
      //   title: "Movimiento 1",
      //   description: "Deposito de salario",
      //   amount: 100,
      //   time: new Date("05-17-2024"),
      // },
      // {
      //   id: 2,
      //   title: "Movimiento 2",
      //   description: "Deposito de honorarios",
      //   amount: 200,
      //   time: new Date("05-17-2024"),
      // },
      // {
      //   id: 3,
      //   title: "Movimiento 3",
      //   description: "Comida",
      //   amount: 500,
      //   time: new Date("05-17-2024"),
      // },
      // {
      //   id: 4,
      //   title: "Movimiento 4",
      //   description: "Colegiatura",
      //   amount: 200,
      //   time: new Date("05-17-2024"),
      // },
      // {
      //   id: 5,
      //   title: "Movimiento 5",
      //   description: "Reparación equipo",
      //   amount: -400,
      //   time: new Date("05-17-2024"),
      // },
      // {
      //   id: 6,
      //   title: "Movimiento 6",
      //   description: "Reparación equipo",
      //   amount: -600,
      //   time: new Date("05-17-2024"),
      // },
      // {
      //   id: 7,
      //   title: "Movimiento 7",
      //   description: "Reparación equipo",
      //   amount: -300,
      //   time: new Date("05-17-2024"),
      // },
      // {
      //   id: 8,
      //   title: "Movimiento 8",
      //   description: "Reparación equipo",
      //   amount: 0,
      //   time: new Date("05-17-2024"),
      // },
      // {
      //   id: 9,
      //   title: "Movimiento 9",
      //   description: "Limpieza",
      //   amount: 300,
      //   time: new Date("04-17-2024"),
      // },
      // {
      //   id: 10,
      //   title: "Movimiento 10",
      //   description: "Desayuno",
      //   amount: 500,
      //   time: new Date("04-17-2024"),
      // },
    ],
  }),
  computed: {
    amounts() {
      const lastDays = this.movements
        .filter((m) => {
          const today = new Date();
          const oldDate = today.setDate(today.getDate() - 30);

          return m.time > oldDate;
        })
        .map((m) => m.amount);

      return lastDays.map((m, i) => {
        const lastMovements = lastDays.slice(0, i + 1);

        return lastMovements.reduce((suma, movement) => {
          return suma + movement;
        }, 0);
      });
    },
    totalAmount() {
      return this.movements.reduce((suma, m) => {
        return suma + m.amount;
      }, 0);
    },
  },
  mounted() {
    const movements = JSON.parse(localStorage.getItem("movements"));
    if (Array.isArray(movements)) {
      this.movements = movements?.map((m) => {
        return { ...m, time: new Date(m.time) };
      });
    }
  },
  methods: {
    create(movement) {
      this.movements.push(movement);
      this.save();
    },
    remove(id) {
      this.movements = this.movements.filter((m) => m.id !== id);
      this.save();
    },
    save() {
      localStorage.setItem("movements", JSON.stringify(this.movements));
    },
    select(el) {
      this.amount = el;
    },
  },
};
</script>
