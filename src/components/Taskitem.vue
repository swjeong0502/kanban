<template>
  <div>
    <li class="task-item">
       <div class="task-item-header">
       
        <div class="assigned-users">
             홍길동(남) - 참여도높음
          <div
            class="task-priority"
            :class="item.priority"
            v-if="showTaskPriority"
            @click="changePriority"
          >Details</div>
          <v-select
            v-model="item.priority"
            ref="vueDropdown"
            :options="['Low','Medium', 'High','Detail']"
            v-if="showTaskPriorityDropdown"
            @search:blur="setNewPriority"
            :clearable="false"
            :closeOnSelect="true"
            class="custom-v-select"
          ></v-select>
        </div>
        <hr style="margin-top:15px; margin-bottom:15px">

      </div>
      
      <div class="task-item-body">
        <p class="task-title"  ><ion-icon name="person-outline" ></ion-icon> 1991-05-02 (만 30세)</p>
        <p class="task-title" ><ion-icon name="call-outline"></ion-icon> 010-5448-1326 <ion-icon name="chatbox-outline" size="small"></ion-icon></p>
        <p class="task-title" ><ion-icon name="calendar-outline" style="color:blue"></ion-icon> 2022-02-09(수) 18:00</p>
               <!-- <textarea type="text" class="form-control task-title" :value="task.title" rows="2"></textarea> -->
      </div>
     <div class="comments-attachments" style="font-style:bold">Memo</div>
      <div class="task-item-footer">
        <div class="comments-attachments">

       예약된 대상자->전화 스크리닝<br>
        - 2022-02-09 18:00 김민걸 -
       
        </div>
      <!-- <div class="task-item-header"> -->
        
         
       

      <!-- </div> -->
      </div>
    </li>
  </div>
</template>

<script>
import store from "./../store/index";
import { mapActions, mapGetters } from "vuex";
import vSelect from "vue-select";
import { Bus } from "./../utils/bus";
import "vue-select/dist/vue-select.css";
import TaskDetailPopup from "./popups/TaskDetailPopup";

export default {
  name: "Taskitem",
  props: ["item", "list", "board"],
  components: {
    "v-select": vSelect,
    TaskDetailPopup
  },
  data() {
    return {
      showTaskPriorityDropdown: false,
      showTaskPriority: true
    };
  },
  watch: {},
  methods: {
    assignUser(user){
      this.item.assignedUsers.push(user)
      console.log(this.item);
      
    },
    changePriority() {
      this.showTaskPriorityDropdown = !this.showTaskPriorityDropdown;
      this.showTaskPriority = !this.showTaskPriority;
      this.$nextTick(() => {
        const input = this.$refs.vueDropdown.$el.querySelector("input");
        input.focus();
      });
    },
    setNewPriority(e) {
      this.showTaskPriorityDropdown = !this.showTaskPriorityDropdown;
      this.showTaskPriority = !this.showTaskPriority;
    },
    openTaskDetailPopoup(item) {
      console.log("clicked");

      Bus.$emit("open-task-detail-popup", item);
    }
  },
  created() {},
  computed: {}
};
</script>

<style scoped lang="scss" >
.assigned-users {
  .user-avatar {
    margin-right: -15px;
  }
}
.assigned-users .add-icon {
  margin-left: 20px;
  cursor: pointer;
}
.custom-v-select {
  font-size: 14px;
}
.assignee-selection .dropdown-item {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  padding: 0.4rem .8rem;
  .user-avatar {
    margin-right: 15px;
  }
  .user-name {
    font-size: 14px;
    font-weight: 400;
    color: rgb(45, 45, 82);
  }
}
</style>
