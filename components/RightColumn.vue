<template>
    <div class="mainBlock--rightColumn">
        <div v-if="selectedUser" class="userProfile">
            <img class="userImg" :src="image" alt="#" />
            <div class="info_block">
                <div class="firstname_and_lastname">{{ selectedUser.name }}</div>
                <div class="assist_block">
                    <span class="title">email: </span>
                    <span class="text">{{ selectedUser.mail }}</span>
                </div>
                <div class="assist_block">
                    <span class="title">phone: </span>
                    <span class="text">{{ selectedUser.phone }}</span>
                </div>
                <div class="desk_block">
                    <div class="deskTitle">О себе</div>
                    <div class="deskription">{{ selectedUser.deskription }}</div>
                </div>
            </div>
        </div>
        <div v-else class="mainBlock__rightColumn--alternatimeText">Выберите сотрудника, чтобы посмотреть его профиль</div>
    </div>
</template>
  
<script>
export default {
    props: {
        selectedUser: {
        type: Object,
        default: null,
        },
        image: {
        type: String,
        required: true,
        },
    },
};
</script>
  
<style lang="scss" scoped>
.mainBlock--rightColumn {
    min-width: 975px;
    max-width: 975px;
    border-left: 1px solid #dededd;

    .userProfile {
        padding: 30px 30px 0 21px;
        display: flex;
        gap: 61px;

        .userImg {
            width: 424px;
            height: 286px;
        }

        .firstname_and_lastname {
            font-weight: 600;
            font-size: 16px;
            line-height: 22.4px;
            color: black;
        }

        .assist_block {
            margin-top: 10px;

            .title {
                font-weight: 600;
                font-size: 14px;
                line-height: 19.6px;
            }

            .text {
                color: #76787d;
            }
        }

        .desk_block {
            margin-top: 20px;

            .deskTitle {
                font-weight: 600;
                font-size: 16px;
                line-height: 22.4px;
            }

            .deskription {
                height: auto;
                width: 439px;
                white-space: normal;
                color: #76787d;
                margin-top: 25px;
            }
        }     
    }

    .mainBlock__rightColumn--alternatimeText {
        position: relative;
        margin-top: 250px;
        display: flex;
        justify-content: center;
    }
}
</style>
