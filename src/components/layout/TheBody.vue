<template>
  <div class="main-container">
    <TheNavbar></TheNavbar>
    <div class="body">
    
    <div class="page-content">
      <div class="page-content-header">
        <div class="page-content-title">Nhân viên</div>
        <div class="page-content-insert-btn">
          <Button
            class="btn-add-epl"
            btnText="Thêm mới nhân viên"
            @click="openFormInsert"
          ></Button>
        </div>
      </div>
      <div class="page-content-body">
        <div class="table-content">
          <div class="header-table">
            
            <div class="m-search-area">
              <div class="m-search-area-content">
                <input
                  type="text"
                  class="m-border-input-search"
                  placeholder="Tìm theo mã, tên nhân viên"
                  v-model="filter.employeeFilter"
                />
                <!-- click vào icon search sẽ search table màn hình  -->
                <div class="m-icon-16 m-icon-search" @click="searchEmployee">
              </div>
              </div>
              <!-- Nút refresh lại dữ liệu -->
              <div class="m-btn-refresh m-icon-24 m-icon-refresh" @click="refreshData"></div>
            </div>
          </div>
          <!-- Phần bảng danh sách nhân viên -->
          <EmployeesTable />
          <!-- Phần paging -->
          <Paging />
        </div>
      </div>
    </div>
    <!-- Phần progress loading -->

    
    <!-- Phần popup form thêm mới nhân viên -->
    <FormDetailEmployee v-if="isShowForm"/>
    <!-- Phần thông báo thêm hoặc sửa hoặc xóa thành công -->
    <Notice ></Notice>
    <!-- Phần Alert cảnh báo  -->
    <Alert></Alert>
  </div>
  </div>

</template>
<script>
import Button from "../../components/base/BaseButton.vue";
import EmployeesTable from "../base/BaseTable.vue";
import Paging from "../../views/PagingEmployeePage.vue";
import FormDetailEmployee from "../../views/FormDetailEmployee.vue";
import Notice from "../../components/base/BaseNotice.vue";
import Alert from "../../views/AlertDialog.vue"
import Gender from "../../enums/gender"
import { mapActions, mapState } from "vuex";
import TheNavbar from "../../components/layout/TheNavbar.vue"

export default {
  name: "TheBody",
  components: {
    TheNavbar,
    EmployeesTable,
    Paging,
    Button,
    FormDetailEmployee,
    Notice,
    Alert
  },
  computed:mapState({
    titleForm: (state)=> state.titleForm,
    isShowForm: (state) => state.isShowForm,
    filter:(state)=>state.filter,
  }),
  created() {},
  data() {
    return {
      employees: [],
    };
  },
  methods: {
    ...mapActions(["toggleForm"]),
    ...mapActions(["toggleProgressLoading"]),
    ...mapActions(["setTitleForm"]),
    ...mapActions(["setDetailEmployee"]),
    ...mapActions(["getEmployees"]),
    ...mapActions(["setFilter"]),
    ...mapActions(["DeleteList"]),
    /**
     * Kích nút "Thêm mới nhân viên" thì mở form thông tin nhân viên
     * Author: DungNP
     */
    openFormInsert() {
      // Reset lai form roi moi mo ra
      const me = this;
      me.toggleProgressLoading();
      me.setDetailEmployee({Gender:(Gender.male)});
      setTimeout(() => {
        me.setTitleForm("Thêm mới nhân viên");
        me.toggleProgressLoading();
        me.toggleForm();
      }, 1000);

    },
    /**
     * Hàm tìm kiếm nhân viên theo keyword
     */
    searchEmployee(){
            const me = this;
            
            me.setFilter({
                pageSize: me.filter.pageSize,
                pageNumber: 1,
                employeeFilter: me.filter.employeeFilter
            });
            me.getEmployees();
        },
    /**
     * Hàm load lại dữ liệu
     * Author: DungNP
     */
    refreshData(){
      const me = this;
      me.toggleProgressLoading();
      setTimeout(() => {
        me.getEmployees();
        me.toggleProgressLoading();
      }, 1000);
    },

  },
};
</script>
<style scoped>
@import url("../../css/layout/body.css");
@import url("../../assets/icofont/icofont.css");
</style>