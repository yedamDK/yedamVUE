<template>
  <div>
    <h1>수두권 미세먼지 초미세먼지 농도</h1>
    <button type="button" @click="getData">데이터 출력</button>
    <table>
      <thead>
        <tr>
          <th>지역</th>
          <th>미세먼지 농도/대기질</th>
          <th>초미세먼지 농도/대기질</th>
          <th>미세먼지 농도/대기질</th>
          <th>초미세먼지 농도/대기질</th>
        </tr>
      </thead>
      <tbody>
        <tr :key="item.idx" v-for="item in dustArray">
          <td>{{ item.MSRSTE_NM }}</td>
          <td>{{ item.PM10 }} / {{ dust(item.PM10) }}</td>
          <td>{{ item.PM25 }} / {{ micro(item.PM25) }}</td>
          <td>
            {{
              item.PM10 > 100 ? item.PM10 + " / 나쁨" : item.PM10 + " / 좋음"
            }}
          </td>
          <td>{{ item.PM25 }} / {{ item.PM25 > 35 ? "나쁨" : "좋음" }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
export default {
  data() {
    return {
      dustArray: {},
    };
  },
  methods: {
    getData() {
      fetch(
        "http://openapi.seoul.go.kr:8088/6d4d776b466c656533356a4b4b5872/json/RealtimeCityAir/1/99"
      )
        .then((res) => res.json())
        .then((data) => {
          console.log(data);
          this.dustArray = data.RealtimeCityAir.row;
        })
        .catch((err) => console.log(err));
    },
    dust(PM10) {
      if (PM10 > 100) {
        return "나쁨";
      } else {
        return "좋음";
      }
    },
    micro(PM25) {
      return PM25 > 35 ? "나쁨" : "좋음";
    },
  },
};
</script>
