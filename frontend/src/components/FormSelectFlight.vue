<template>
  <v-container grid-list-xl>
    <Information :receiveParams="receiveData"/>
    <p class="title-page-2">Chuyến bay chiều đi</p>
    <div class="table-flight">
      <v-layout class="table-header">
        <v-flex xs6 md4 class="item">Thời gian</v-flex>
        <v-flex xs6 md2 class="item">Điểm dừng</v-flex>
        <v-flex xs6 md3 class="item">Phổ thông</v-flex>
        <v-flex xs6 md3 class="item">Thương gia</v-flex>
      </v-layout>
      <v-layout
        class="table-items"
        v-for="flight in flightSearch"
        @mouseover="onHover(flight._id)"
        :key="flight._id"
      >
        <v-flex xs6 md4>
          <!-- <span>dataReturnDay</span> -->
          <br>
          <span>{{flight.flight_number}} {{flight.planes_code}}</span>
          <br>
          <span>{{flight !=null ? (new Date(flight.datetime * 1000).toISOString().slice(0,10)) : ''}}</span>
        </v-flex>
        <v-flex xs6 md2>Bay thẳng</v-flex>
        <v-flex xs6 md3 class="type">
          <div class="title">ECONOMIC</div>
          <p>Từ</p>
          <div
            class="price"
          >{{flight.price_economy != null ? flight.price_economy.toLocaleString('vi') : ''}} VND</div>
          <v-btn v-if="showBtn == flight._id" @click="selectType('economic', flight)">Chọn</v-btn>
        </v-flex>
        <v-flex xs6 md3 class="type">
          <div class="title">BUSSINESS</div>
          <p>Từ</p>
          <div class="price">{{flight.price_bussiness != null ? flight.price_bussiness.toLocaleString('vi') : ''}} VND</div>
          <v-btn v-if="showBtn == flight._id" @click="selectType('bussiness', flight)">Chọn</v-btn>
        </v-flex>
      </v-layout>
    </div>
    <v-flex md6 class="back-button">
      <v-btn round outline color="info" @click="backToPre">Quay lại</v-btn>
    </v-flex>
  </v-container>
</template>

<script>
import Information from "@/components/Information";
export default {
  components: {
    Information
  },
  props: {
    receiveData: null,
    flightSearch: Array
  },
  data: () => ({
    showBtn: 1,
    type: null
  }),
  computed: {
    dataReturnDay() {
      return this.formatDate(this.flight.datetime);
    }
  },
  created() {},
  methods: {
    onHover(key) {
      this.showBtn = key;
    },
    selectType(type, flight) {
      flight.type = type
      this.$emit("selectType", flight);
    },
    backToPre() {
      this.$emit("backToPrePage");
    }
  },
  formatDate(date) {
    if (!date) return null;

    const [year, month, day] = date.split("-");
    return `${day} / ${month} / ${year}`;
  }
};
</script>

<style>
.type .title p {
  text-align: justify;
}
.type .title .price {
  text-align: justify;
}
.back-button {
  margin-top: 30px;
}
.table-header {
  background-color: #aac8dc;
  font-weight: bold;
  font-size: 16px;
  width: 100%;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  margin: 0 auto !important;
}
.table-items {
  width: 100%;
  margin: 0 auto !important;
}
.table-items:hover {
  background-color: white !important;
}
.table-items:nth-child(even) {
  background-color: #b3d0dc8f;
}
.table-items:nth-child(odd) {
  background-color: #d6eaf8;
}
.type {
  margin: 10px;
  border-radius: 4px;
}
.type p {
  margin: 0 !important;
  padding-left: 10px;
}
.type {
  border-left: 10px solid #e59866;
  background-color: #efefef;
}
.group {
  display: flex;
  flex: 1;
}
.title-page-2 {
  padding-top: 20px;
  width: 100%;
  color: #cc840f;
  font-weight: bold;
  font-size: 20px;
  text-align: left;
}
table {
  display: block;
}
</style>
