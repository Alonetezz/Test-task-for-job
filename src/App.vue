<template>
  <div>
    <Header />
    <div class="mainBlock">
      <Sidebar 
        :searchQuery="searchQuery" 
        :selectedUser="selectedUser"
        @update-search="updateSearch" 
        @select-user="setSelectedUser" 
      />
      <div class="mainBlock--rightColumn">
        <div v-if="selectedUser" class="userProfile">
          <img class="userImg" :src="image" alt="#" />
          <div class="info_block">
            <div class="firstname_and_lastname">{{ selectedUser.name }}</div>
            <div class="assist_block">
              <span class="title">email:  </span>
              <span class="text">{{ selectedUser.mail }}</span>
            </div>
            <div class="assist_block">
              <span class="title">phone:  </span>
              <span class="text">{{ selectedUser.phone }}</span>
            </div>

            <div class="desk_block">
              <div class="deskTitle">О себе</div>
              <div class="deskription">{{ selectedUser.deskription }}</div>
            </div>
          </div>
        </div>
        <div v-else class="mainBlock__rightColumn--alternatimeText">
          Выберите сотрудника, чтобы посмотреть его профиль
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Header from '../components/Header.vue';
import Sidebar from '../components/Sidebar.vue';

import image from '/capy.jpg';

export default {
  data() {
    return {
      image: image,
      searchQuery: '',
      selectedUser: null, // Для хранения выбранного пользователя
      users: [
        { id: 0, username: 'anna22anna', name: 'Anna Ivanovna', mail: 'anna22anna@gmail.com', phone: '1111112222222333333', deskription: 'Это описание первого пользователя.' },
        { id: 1, username: 'ivanivan', name: 'Ivan Ivanov', mail: 'ivan_ivanov_2000@gmail.com', phone: '33333333222222222111111111', deskription: 'Это описание второго пользователя.' },
        { id: 2, username: 'artemka', name: 'Artem Petrov', mail: 'artemp_98@mail.com', phone: '0123456789', deskription: 'Это описание третьего пользователя.' },
        { id: 3, username: 'seregapirat', name: 'Sergey Malyarov', mail: 'pirat_sergey@mail.com', phone: '0123456789', deskription: 'Пасхалко' },
        { id: 4, username: 'starybog', name: 'Vladislav Levenets', mail: 'vlad_lev@gmail.com', phone: '9876543210', deskription: 'Пасхалко' },
      ],
    };
  },

  watch: {
    searchQuery: 'resetSelectedUser',
  },
  
  methods: {
    updateSearch(query) {
      this.searchQuery = query;
    },
    setSelectedUser(user) {
      this.selectedUser = user; // Обновляем выбранного пользователя
      console.log(JSON.stringify(user, null, 2)); // Выводим информацию о пользователе в формате JSON
    },
    resetSelectedUser() {
      // Проверяем, находится ли выбранный пользователь в отфильтрованном списке
      const filteredUsers = this.filteredUsers;
      
      // Сбрасываем выбранного пользователя, если его нет в отфильтрованном списке
      if (!filteredUsers.find(user => user.id === this.selectedUser?.id) || this.searchQuery === '') {
        this.selectedUser = null; // Сбрасываем выбранного пользователя
      }
    },
  },
  
  computed: {
    filteredUsers() {
      const queries = this.searchQuery
        .toLowerCase()
        .split(',')
        .map((query) => query.trim());

      // Фильтруем пользователей по любому из условий, но сортируем по имени
      return this.users.filter((user) =>
        queries.some(
          (query) =>
            user.username.toLowerCase().includes(query) ||
            user.name.toLowerCase().includes(query) ||
            user.id.toString().includes(query)
        )
      ).sort((a, b) => a.name.localeCompare(b.name));
    },
  },

  components: {
    Header,
    Sidebar,
  },
};
</script>




<style lang="scss">
html {
  width: 100%;
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;
}

* {
  font-family: 'Montserrat', sans-serif;
}

.mainBlock {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  min-height: 589px;
  max-height: 589px;
  margin-top: 26px;
  width: 1266px;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  display: flex;
}

.mainBlock--rightColumn {
  min-width: 975px;
  max-width: 975px;
  border-left: 1px solid #DEDEDD;
}

.userProfile {
  padding: 30px 30px 0 21px;
  display: flex;
  gap: 61px;
  



  .userImg {
    width: 424px;
    height: 286px;
  }

  .info_block {

  }
}

.firstname_and_lastname {
  font-weight: 600;
  font-size: 16px;
  line-height: 22.4px;
  color: black;
}

.mainBlock__rightColumn--alternatimeText {
  position: relative;
  margin-top: 250px;
  display: flex;
  justify-content: center;
}

.assist_block {
  margin-top: 10px;
}

.title{
  font-weight: 600;
  font-size: 14px;
  line-height: 19.6px;
}

.text{
 color: #76787D;;
}

.desk_block {
  margin-top: 20px;
}

.deskTitle {
  font-weight: 600;
  font-size: 16px;
  line-height: 22.4px;
}

.deskription {
  height: auto;
  width: 439px;
  // word-break: break-all;
  white-space: normal;
  color: #76787D;
  margin-top: 25px;
}
</style>
