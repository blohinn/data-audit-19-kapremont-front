<template>
  <div class="modal">
    <div class="container-fluid">
      <div class="scroll-zone">
        <div class="row">
          <div class="col-12 mt-2">
            <button
              v-on:click="closeModal"
              type="button"
              class="btn btn-link p-0 float-right"
            >
              Закрыть
            </button>
          </div>
          <div class="col-12">
            <h2>{{ data.address }}</h2>
            <br />
            <h4>Капитальный ремонт 2018:</h4>
            <table class="table">
              <thead class="thead-dark">
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">Вид работы</th>
                  <th scope="col">Стоимость (план, тыс.р.)</th>
                  <th scope="col">Стоимость (факт, тыс.р.)</th>
                  <th scope="col">Заказчик</th>
                  <th scope="col">Исполнитель</th>
                  <th scope="col">Договор (дата)</th>
                  <th scope="col">Акт-прием (дата)</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(job, i) in data.jobs">
                  <th scope="row">{{ i + 1 }}</th>
                  <td>{{ job.job }}</td>
                  <td>{{ job.price.plan }}</td>
                  <td>{{ job.price.total }}</td>
                  <td>{{ job.customer.name }}</td>
                  <td>{{ job.performer.name }}</td>
                  <td>{{ job.contract.date_conclusion }}</td>
                  <td>{{ job.contract.date_act_accepting }}</td>
                </tr>
              </tbody>
            </table>

            <hr />
          </div>

          <div class="col-8">
            <h3>Положительные признаки:</h3>
            <table class="table">
              <thead class="thead-dark">
                <tr>
                  <th scope="col">Признак</th>
                  <th scope="col">Соблюден (хорошо)</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <th scope="row">
                    Фактическая цена оказалась дешевле, чем планировалось (была
                    конкуренция)
                  </th>
                  <th scope="row" v-bind:class="getBgClassByFeature(data.features.positive.total_price_lt_plan_price)">
                    {{
                      featureToText(
                        data.features.positive.total_price_lt_plan_price
                      )
                    }}
                  </th>
                </tr>
              </tbody>
            </table>

            <h3>Отрицательные признаки:</h3>
            <table class="table">
              <thead class="thead-dark">
                <tr>
                  <th scope="col">Признак</th>
                  <th scope="col">Соблюден (плохо)</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <th scope="row">Нет информации об исполнителе</th>
                  <th scope="row" v-bind:class="getBgClassByFeature(data.features.negative.performer_info_not_available, true)">
                    {{
                      featureToText(
                        data.features.negative.performer_info_not_available
                      )
                    }}
                  </th>
                </tr>
                <tr>
                  <th scope="row">
                    Итоговая цена оказалась равной (или больше), чем
                    планировалось
                  </th>
                  <th scope="row" v-bind:class="getBgClassByFeature(data.features.negative.total_price_gte_plan_price, true)">
                    {{
                      featureToText(
                        data.features.negative.total_price_gte_plan_price
                      )
                    }}
                  </th>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Modal",
  props: ["data"],
  methods: {
    closeModal() {
      this.$emit("modalClosed");
    },
    featureToText(featureValue) {
      console.log(featureValue);
      if (featureValue === null) {
        return "Нет";
      }
      if (featureValue === true) {
        return "Да";
      }
      if (featureValue === false) {
        return "Нет";
      }

      return "Нет";
    },
    getBgClassByFeature(featureValue, reverse = false) {
      if (reverse) {
        if (featureValue === null) {
          return "bg-success";
        }
        if (featureValue === true) {
          return "bg-danger";
        }
        if (featureValue === false) {
          return "bg-success";
        }
      } else {
        if (featureValue === null) {
          return "bg-danger";
        }
        if (featureValue === true) {
          return "bg-success";
        }
        if (featureValue === false) {
          return "bg-danger";
        }
      }
    }
  }
};
</script>

<style>
.modal {
  display: block;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 9999;

  height: 100%;
  width: 100%;

  background-color: white;
  opacity: 0.9;
}

.scroll-zone {
  overflow-y: scroll;
  height: 700px;
}
</style>
