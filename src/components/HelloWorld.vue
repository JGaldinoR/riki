<template>
  <div >
   <vui-dtable 
	id="dt1" 
	:rows="dt1.rows" 
	:headers="dt1.headers"
	border
	pageSize="10"
	scroll
/>
<br>
<br>

<GChart
    type="ColumnChart"
    :data="chartData"
    :options="chartOptions"
  />

</vui-accordion>

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      dt1: {
	headers: [
    { id: 0, value: 'id', label: 'ID', sortable: false, selected: true },
		{ id: 1, value: 'name', label: 'Name', sortable: false, selected: true },
		{ id: 2, value: 'age', label: 'Age', sortable: false, selected: true },
		{ id: 3, value: 'breed', label: 'Breed', sortable: false, selected: true },
		{ id: 4, value: 'gender', label: 'Gender', sortable: false, selected: true }
  ],
	rows: [
		{ "id":0, "name":"Alpha", "age":2, "breed":"Calico", "gender":"male" },
		{ "id":1, "name":"Beta", "age":1, "breed":"Siamese", "gender":"female" },
		{ "id":2, "name":"Gamma", "age":6, "breed":"Calico", "gender":"female" },
		{ "id":3, "name":"Delta", "age":5, "breed":"Bengal", "gender":"male" },
    { "id":4, "name":"Epsilon", "age":2, "breed":"Calico", "gender":"female" },
    { "id":4, "name":"Epsilon", "age":2, "breed":"Calico", "gender":"female" },
    { "id":4, "name":"Epsilon", "age":2, "breed":"Calico", "gender":"female" },
    { "id":4, "name":"Epsilon", "age":2, "breed":"Calico", "gender":"female" },
    { "id":4, "name":"Epsilon", "age":2, "breed":"Calico", "gender":"female" },
    { "id":4, "name":"Epsilon", "age":2, "breed":"Calico", "gender":"female" },
    { "id":4, "name":"Epsilon", "age":2, "breed":"Calico", "gender":"female" },
    { "id":4, "name":"Epsilon", "age":2, "breed":"Calico", "gender":"female" },
    { "id":4, "name":"Epsilon", "age":2, "breed":"Calico", "gender":"female" },
	],
	genders: ['female', 'male'],
	breeds: ['Persian', 'Siamese', 'Bengal', 'Calico', 'Ragdoll', 'Domestic']
},
chartData: [
        ['Year', 'Sales', 'Expenses', 'Profit'],
        ['2014', 1000, 400, 200],
        ['2015', 1170, 460, 250],
        ['2016', 660, 1120, 300],
        ['2017', 1030, 540, 350]
      ],
      chartOptions: {
        chart: {
          title: 'Company Performance',
          subtitle: 'Sales, Expenses, and Profit: 2014-2017',
        }
      },
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
