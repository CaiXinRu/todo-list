<template>
  <div class="body">
    <div class="container">
      <h1>TODO LIST</h1>
      <div class="card input">
        <input
          type="text"
          placeholder="請輸入待辦事項"
          v-model="newItem"
          @keypress.enter="addItem"
        />
        <a href="#" class="btn_add" @click="addItem">+</a>
      </div>
      <div class="card card_list">
        <ul class="tab">
          <li
            v-for="(tab, index) in tabs"
            :key="index"
            :class="{ active: toggleStatus === tab.value }"
            @click="toggleStatus = tab.value"
          >
            {{ tab.label }}
          </li>
        </ul>
        <div class="cart_content">
          <!-- <ul class="list">
            <li v-for="item in filteredList" :key="item.id" :data-id="item.id">
              <label class="checkbox" for="">
                <input type="checkbox" v-model="item.checked" />
                <span
                  :style="{
                    'text-decoration': item.checked ? 'line-through' : 'none'
                  }"
                  >{{ item.content }}</span
                >
              </label>
              <a class="delete" @click="deleteItem(item.id)"></a>
            </li>
          </ul> -->
          <todo-list
            :filteredList="filteredList"
            @delete="deleteItem"
          ></todo-list>
          <div class="list_footer">
            <p v-if="!getUndoneItemsCount">
              <span>目前沒有待辦項目</span>
            </p>
            <p v-else>
              <span>{{ getUndoneItemsCount }}</span
              >個待完成項目
            </p>
            <a style="cursor: pointer" @click="deleteAllCompleted"
              >清除已完成項目</a
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TodoList from '@/components/TodoList.vue'

export default {
  name: 'HomeView',
  data () {
    return {
      newItem: '',
      data: [],
      toggleStatus: 'all',
      tabs: [
        { label: '全部', value: 'all' },
        { label: '待完成', value: 'undone' },
        { label: '已完成', value: 'done' }
      ]
    }
  },
  components: {
    TodoList
  },
  computed: {
    filteredList () {
      if (this.toggleStatus === 'all') {
        return this.data
      } else if (this.toggleStatus === 'undone') {
        return this.data.filter((item) => !item.checked)
      } else {
        return this.data.filter((item) => item.checked)
      }
    },
    getUndoneItemsCount () {
      return this.data.filter((item) => !item.checked).length
    }
  },
  methods: {
    addItem () {
      if (this.newItem.trim() === '') {
        // alert('請填寫待辦事項！')
        this.$swal({
          title: '請填寫待辦事項！',
          confirmButtonColor: '#ffd370',
          focusConfirm: false
        })
        return
      }
      const obj = {
        content: this.newItem,
        checked: false,
        id: new Date().getTime()
      }
      this.data.unshift(obj)
      this.newItem = ''
      this.toggleStatus = ''
      this.toggleStatus = 'all'
    },
    deleteItem (id) {
      const index = this.data.findIndex((item) => item.id === id)
      this.$swal({
        title: '確認刪除此項目？',
        text: '刪除後將無法再進行復原！',
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: 'rgb(134 190 249)',
        cancelButtonColor: 'rgb(249 134 134)',
        confirmButtonText: '是的，請刪除！',
        cancelButtonText: '取消',
        focusConfirm: false
      }).then((result) => {
        if (result.isConfirmed) {
          this.data.splice(index, 1)
          this.$swal.fire('刪除！', '此項目已被刪除。', '成功')
        }
      })
    },
    deleteAllCompleted () {
      const undoneItems = this.data.filter((item) => item.checked)
      if (undoneItems.length === 0) {
        this.$swal({
          title: '沒有已完成的項目！',
          icon: 'info',
          confirmButtonColor: '#3085d6',
          confirmButtonText: '好的'
        })
        return
      }
      this.$swal({
        title: '是否要清除已完成項目？',
        text: '清除後將無法再進行復原！',
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: 'rgb(134 190 249)',
        cancelButtonColor: 'rgb(249 134 134)',
        confirmButtonText: '是的，請清除！',
        cancelButtonText: '取消',
        focusConfirm: false
      }).then((result) => {
        if (result.isConfirmed) {
          this.data = this.data.filter((item) => !item.checked)
          this.$swal.fire('清除！', '完成項目已全被清除。', '成功')
        }
      })
    }
  }
}
</script>

<style lang="scss">
@import '@/assets/base.scss';

h1 {
  text-align: center;
  font-size: 3rem;
  margin-bottom: 1.5rem;
  font-family: 'Baloo Tamma 2';
  letter-spacing: 0.5rem;
  font-weight: bold;
  @media (max-width: 575px) {
    font-size: 2rem;
    margin-bottom: 1rem;
  }
}
.container {
  margin: 3rem auto 1.5rem auto;
  padding: 0 12px;
  width: 500px;
  @media (max-width: 575px) {
    margin-top: 1.5rem;
  }
}
.card {
  margin-bottom: 0.5rem;
  max-width: 100%;
  padding: 1rem;
  border-radius: 10px;
  background: #fff;
  box-shadow: 0 0 15px 0 rgba(0, 0, 0, 0.15);
}
input[type='text'] {
  width: 100%;
  border: 0;
  outline: 0;
  font-size: 1rem;
  padding-right: 1rem;
  color: $dark;
  &::placeholder {
    color: $gray;
  }
}
.input {
  padding: 4px 4px 4px 1rem;
  display: flex;
}
.btn_add {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  width: 40px;
  height: 40px;
  border-radius: 10px;
  background: $dark;
  color: #fff;
  font-size: 2rem;
  text-decoration: none;
  cursor: pointer;
}

// card_list
.card_list {
  padding: 0;
}

.tab {
  list-style: none;
  display: flex;
  text-align: center;
  color: $gray;
  font-size: 14px;
  li {
    padding: 1rem;
    width: 100%;
    border-bottom: 2px solid $light;
    &.active {
      border-bottom: 2px solid $dark;
      color: $dark;
      font-weight: bold;
    }
  }
}
.cart_content {
  padding: 0.5rem 1rem 1rem 1rem;
  @media (max-width: 575px) {
    padding: 0.5rem 1rem 0.5rem 0.5rem;
  }
}
.list {
  list-style: none;
  li {
    position: relative;
    padding-right: 2rem;
    @media (max-width: 575px) {
      padding-right: 0;
    }
    a.delete {
      position: absolute;
      opacity: 0;
      right: 0;
      top: 50%;
      transform: translateY(-50%);
      text-decoration: none;
      color: $dark;
      display: block;
      width: 1rem;
      height: 1rem;
      background: #000;
      background-image: url('https://i.imgur.com/7Q4RjFx.jpg');
      background-size: contain;
      cursor: pointer;
      @media (max-width: 575px) {
        opacity: 1;
        width: 12px;
        height: 12px;
      }
    }
    &:hover a.delete {
      opacity: 1;
    }
  }
}
.list_footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1.5rem 2rem 1rem 0.5rem;
  font-size: 14px;
  a {
    color: $gray;
    text-decoration: none;
  }
  @media (max-width: 575px) {
    padding: 1.5rem 0 1rem 0.5rem;
  }
}

.checkbox {
  position: relative;
  user-select: none;
  width: 100%;
  display: block;
  padding-left: 44px;
  cursor: pointer;
  span {
    display: block;
    padding: 1rem 0;
    border-bottom: 1px solid #eee;
    line-height: 1.5;
    @media (max-width: 575px) {
      padding-right: 1rem;
    }
  }
  input {
    position: absolute;
    top: 0;
    left: 0;
    opacity: 0;
    cursor: pointer;
    display: block;
    height: 100%;
    width: 100%;
    margin: 0;
  }
  span::before {
    content: '';
    position: absolute;
    left: 0.5rem;
    top: 50%;
    transform: translateY(-50%) scale(1);
    height: 20px;
    width: 20px;
    border-radius: 5px;
    border: 1px solid $dark;
    pointer-events: none;
    transition: 0.3s ease;
  }
  span::after {
    content: '';
    position: absolute;
    left: 14px;
    top: 27%;
    transform: rotate(45deg);
    height: 15px;
    width: 0.5rem;
    border-radius: 1px;
    border-bottom: 3px solid $default;
    border-right: 3px solid $default;
    pointer-events: none;
    opacity: 0;
    transition: 0.3s ease;
  }
  input:checked {
    ~ span {
      color: $gray;
      text-decoration: line-through;
    }
    ~ span::before {
      border-color: transparent;
      transform: translateY(-50%) scale(0);
    }
    ~ span::after {
      opacity: 1;
    }
  }
}
</style>
