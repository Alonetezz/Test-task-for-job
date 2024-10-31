<template>
  <div class="sidebar">
    <div class="serachStuff">Поиск сотрудников</div>
    <input
      v-model="localSearchQuery"
      class="searchString"
      type="text"
      placeholder="Введите id или имя"
      @input="onSearchInput"
    />
    <div>
      <div class="results">Результаты</div>
      <div v-if="localSearchQuery === ''">
        <div class="searchResults">Начните поиск</div>
      </div>
      <div class="searchResults" v-else-if="filteredUsers.length === 0">Нет результатов</div>
      <div v-else>
        <div v-if="isLoading" class="preloader">Загрузка...</div>
        <div
          v-else
          class="outputResult" 
          v-for="user in filteredUsers" 
          :key="user.id" 
          @click="selectUser(user)"
          :class="{ highlighted: isSelectedUser(user) }"
        >
          <img :src="image" alt="#" class="img"/>
          <div class="otherInfo">
            <span id="firstObj">{{ user.username }}</span>
            <span id="secondObj">{{ user.mail }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import image from '/capy2.jpg';

export default {
  props: {
    searchQuery: {
      type: String,
      required: true,
    },
    selectedUser: {
      type: Object,
      default: null,
    },
  },

  data() {
    return {
      image: image,
      isLoading: false, // Для отслеживания состояния загрузки
    };
  },
  
  computed: {
    localSearchQuery: {
      get() {
        return this.searchQuery;
      },
      set(value) {
        this.$emit('update-search', value);
      },
    },
    filteredUsers() {
      this.isLoading = true; // Включаем прелоадер перед началом фильтрации
      const queries = this.localSearchQuery
        .toLowerCase()
        .split(',')
        .map((query) => query.trim());

      // Фильтруем пользователей по любому из условий, но сортируем по имени
      const result = this.$parent.users
        .filter((user) =>
          queries.some(
            (query) =>
              user.username.toLowerCase().includes(query) ||
              user.name.toLowerCase().includes(query) ||
              user.id.toString().includes(query)
          )
        )
        .sort((a, b) => a.name.localeCompare(b.name));

      this.isLoading = false; // Выключаем прелоадер после завершения фильтрации
      return result;
    },
  },

  methods: {
    selectUser(user) {
      this.$emit('select-user', user);
    },
    isSelectedUser(user) {
      return this.selectedUser && this.selectedUser.id === user.id;
    },
  },
};
</script>

<style lang="scss" scoped>
* {
  box-sizing: border-box;
}

.sidebar {
  position: relative;
  display: flex;
  flex-direction: column;
  min-height: 100%;
  padding: 27px 23px 0 23px;
  width: 291px;
  overflow-y: auto;

  &::-webkit-scrollbar {
    width: 4px; 
  }
  &::-webkit-scrollbar-thumb {
    background-color: #ee696d;
    border-radius: 4px;
  }

  .serachStuff {
    font-weight: 600;
    font-size: 16px;
    line-height: 22.4px;
    color: #333333;
  }

  .searchString {
    margin-top: 22px;
    width: 240px;
    border-radius: 8px;
    border: 2px solid #e9ecef;
    padding: 16px 24px 16px 24px;
    font-weight: 400;
    font-size: 14px;
    color: #76787D;
    line-height: 17px;
  }

  .results {
    margin-top: 22px;
    font-weight: 600;
    font-size: 17px;
    color: #333333;
    line-height: 22.4px;
  }

  .searchResults {
    margin-top: 22px;
    font-weight: 400;
    font-size: 14px;
    color: #333333;
    line-height: 17px;
  }

  .outputResult {
    display: flex;
    width: 240px;
    height: 70px;
    margin-top: 20px;
    margin-bottom: 20px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
    border-radius: 10px;
    transition: background-color 0.3s, box-shadow 0.3s;

    &.highlighted {
      background-color: #E0E0E0;
      box-shadow: none;
      border: 1px #E0E0E0 solid;
    }
  }

  .img {
    height: 70px;
    width: 70px;
    background-color: white;
    border-radius: 10px 0px 0px 10px;
    border-right: 1px solid #E0E0E0;
  }

  .otherInfo {
    padding: 15px 15px 0 15px;
    display: flex;
    flex-direction: column;
    font-size: 14px;
    gap: 5px;
    position: relative;
    overflow: hidden;

    #firstObj, #secondObj {
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }
    #secondObj {
      color: #76787D;
    }
  }
}
</style>
