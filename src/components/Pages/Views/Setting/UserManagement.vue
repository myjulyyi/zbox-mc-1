<template>
  <div>
      <card>
        <el-row :gutter="10">
          <form @submit.prevent="changePage()">
            <el-col :span="5">
              <div class="form-group">
                <label>아이디</label>
                <el-input v-model="form.searchId" placeholder="아이디"></el-input>
              </div>
            </el-col>
            <el-col :span="5">
              <div class="form-group">
                <label>이름</label>
                <el-input v-model="form.searchName" placeholder="이름"></el-input>
              </div>
            </el-col>
            <el-col :span="14">
              <div class="pull-right-top24">
                <el-button type="primary" native-type="submit" icon="el-icon-search">검색</el-button>
              </div>
            </el-col>
          </form>
        </el-row>
        <el-table id="UserListTable"
                  style="width: 100%; margin-bottom: 10px;"
                  :data="$store.state.admin.admins"
                  :header-cell-style="tableRowStyle"
                  ref="UserListTable">
          <el-table-column
            label="이름"
            prop="adminName">
          </el-table-column>
          <el-table-column
            label="아이디"
            prop="adminId">
          </el-table-column>
          <el-table-column
            label="등록일">
            <template slot-scope="props">
              {{ props.row.regDate | moment('YYYY-MM-DD')}}
            </template>
          </el-table-column>
          <el-table-column
            label="관리"
            prop="goods_name">
            <template slot-scope="props">
              <el-button type="success" icon="el-icon-edit" @click="handleEdit(props.$index, props.row)">
              </el-button>
            </template>
          </el-table-column>
        </el-table>
        <el-row>
          <el-col :span="24">
            <div class="pull-right">
              <el-button type="primary" icon="el-icon-plus" v-on:click="register">사용자 등록</el-button>
            </div>
          </el-col>
        </el-row>
      </card>
  </div>
</template>

<script>
  import Vue from 'vue'
  import {Select, DatePicker, Form, FormItem, Table, TableColumn, Button, Row, Col, Input, Radio, Checkbox, Progress, TabPane, Tabs} from 'element-ui'
  import {Card} from 'src/components/UIComponents'
  import VueMoment from 'vue-moment'

  Vue.use(VueMoment)

  export default {
      name: "UserManagement",
    components: {
      elCard: Card,
      elTabPane: TabPane,
      elTabs: Tabs,
      elButton : Button,
      elRadio : Radio,
      elForm: Form,
      elFormItem : FormItem,
      elCheckbox: Checkbox,
      elInput : Input,
      elTable : Table,
      elTableColumn : TableColumn,
    },
    created: function () {
      this.$store.dispatch('common/setMenuTitle', "사용자관리");
      this.changePage();
    },
    methods: {
      tableRowStyle() {
        return this.$store.state.common.tableHeaderCellStyle;
      },
      changePage() {
        let param = {
          searchId: this.form.searchId,
          searchName: encodeURI(encodeURIComponent(this.form.searchName)),
        };
        this.$store.dispatch('admin/findAll', param);
      },
      handleEdit(index, row) {
        Vue.router.push({name: 'UserManagementRegistration', params: {adminId: row.adminId}});
      },
      register() {
        Vue.router.push({name: 'UserManagementRegistration'})
      },
    },
    computed: {
    },
    data() {
      return {
        activeName: 'security1',
        form: {
          searchId: '',
          searchName: ''
        },
      }
    }
  }
</script>

<style scoped>

</style>
