<template>
  <div class="TheTable-wrapper">
    <input class="TheTable-input" type="text" placeholder="Введите имя тега" v-model="searchInput">
    <table>
      <thead>
        <tr>
          <th>ID тега</th>
          <th>Имя тега</th>
          <th v-for="idx in countOfAttrs" :key="idx">
            <button class="sort-button" :class="{
              'sorted': idx === sortIndex && sortDirection, 'asc': idx === sortIndex && sortDirection === 'asc',
              'desc': idx === sortIndex && sortDirection === 'desc'
            }" @click="sortByAttr(idx)">
              Атрибут {{ idx }}
            </button>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="tag in filteredTableData" :key="tag.tag_id">
          <td>{{ tag.tag_id }}</td>
          <td>{{ tag.tag_name }}</td>
          <td v-for="attribute in tag.tag_attributes" :key="attribute.attribute_id">
            {{ attribute.attribute_value }}
          </td>
          <td v-for="idx in countOfAttrs - tag.tag_attributes.length" :key="idx"></td>
        </tr>
      </tbody>
    </table>
    <p v-if="filteredTableData.length == 0">Ничего не найдено</p>
  </div>
</template>

<script>
export default {
  name: "TheTable",
  props: {
    tableData: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      searchInput: '',
      sortIndex: null,
      sortDirection: null,
    };
  },
  computed: {
    countOfAttrs() {
      let result = this.tableData.reduce((acc, item) => acc < item.tag_attributes.length ? item.tag_attributes.length : acc, 0);
      return result;
    },
    filteredTableData() {
      let filteredData = this.tableData.filter(tag => tag.tag_name.toLowerCase().includes(this.searchInput.toLowerCase()));
      if (this.sortDirection) {
        let sortIndex = this.sortIndex - 1;
        let sortedData = filteredData.sort(
          (tagA, tagB) => {
            let a = tagA.tag_attributes[sortIndex] ? tagA.tag_attributes[sortIndex].attribute_value : '�';
            let b = tagB.tag_attributes[sortIndex] ? tagB.tag_attributes[sortIndex].attribute_value : '�';
            if (this.sortDirection === 'desc') {
              return b.localeCompare(a);
            }
            return a.localeCompare(b);
          }
        );
        return sortedData;
      }
      else {
        return filteredData;
      }
    },
  },
  methods: {
    sortByAttr(index) {
      if (this.sortIndex === index) {
        switch (this.sortDirection) {
          case null:
            this.sortDirection = 'asc';
            break;
          case 'asc':
            this.sortDirection = 'desc';
            break;
          case 'desc':
            this.sortDirection = null;
            break;
        }
      } else {
        this.sortDirection = 'asc'
      }
      this.sortIndex = index;
    },
  }
};
</script>

<style scoped>
.TheTable-wrapper {
  display: flex;
  flex-direction: column;
  gap: 20px;
  align-items: center;
  padding: 50px;
}

.TheTable-input {
  width: 200px;
  height: 20px;
}

table {
  border-collapse: collapse;
}

th,
td {
  width: 200px;
  height: 30px;
  border: 1px solid black;
  text-align: center;
}

th {
  font-weight: bold;
  font-size: 14px;
  padding: 0;
}

.sort-button {
  cursor: pointer;
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%;
  border: none;
  font-weight: bold;
  font-size: 14px;
}

.sorted {
  background-color: antiquewhite;
}

.asc::after {
  content: ' ↓';
}

.desc::after {
  content: ' ↑';
}
</style>
