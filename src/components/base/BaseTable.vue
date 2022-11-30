<template>
  <div class="btn-delete-all">
              <Button
                class="btn-add-epl"
                id="btn-delete"
                btnDisable="1"
                btnText="Xóa"
                @click="abcd()"
              ></Button>
            </div>
            <div class="buttonxoa">
              <button @click="DeleteList()"></button>
            </div>
  <div class="wrap-table">

    <ProgressLoading></ProgressLoading>
    <table class="m-table" id="listEmployees">
      
      <thead>
        <tr>
          <th class="text-align-center">
            <input id="choose-all" type="checkbox" />
          </th>
          <th
            class="text-align-left"
            fieldname="EmployeeCode"
            style="min-width: 140px"
          >
            Mã nhân viên
          </th>
          <th
            class="text-align-left"
            fieldname="EmployeeName"
            style="min-width: 200px"
          >
            Tên nhân viên
          </th>
          <th
            class="text-align-left"
            fieldname="Gender"
            style="min-width: 130px"
          >
            Giới tính
          </th>
          <th
            class="text-align-center"
            style="min-width: 120px"
            fieldname="DateOfBirth"
          >
            Ngày sinh
          </th>
          <th
            class="text-align-left"
            title="Số chứng minh nhân dân"
            fieldname="IdentityNumber"
            style="min-width: 130px"
          >
            Số CMND
          </th>
          <th
            class="text-align-left"
            style="min-width: 150px"
            fieldname="EmployeePosition"
          >
            Chức danh
          </th>
          <th
            class="text-align-left"
            fieldname="DepartmentName"
            style="min-width: 150px"
          >
            Tên đơn vị
          </th>
          <th
            class="text-align-left"
            style="min-width: 130px"
            fieldname="BankAccountNumber"
          >
            Số tài khoản
          </th>
          <th
            class="text-align-left"
            style="min-width: 150px"
            fieldname="BankName"
          >
            Tên ngân hàng
          </th>
          <th
            class="text-align-left"
            style="min-width: 200px"
            fieldname="BankBranchName"
          >
            Chi nhánh ngân hàng
          </th>
          <th class="text-align-center" style="min-width: 110px">Chức năng</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in employees" :key="index" @dblclick="openDetailFormEmployee(item)" >
          <td class="text-align-center">
            <input type="checkbox" @click="DeleteList(item)" :attributeCheck="item.EmployeeCode" ref="checkbox"  name="choose" class="m-input-checkbox" />
          </td>
          <td class="text-align-left">{{ item.EmployeeCode }}</td>
          <td class="text-align-left">{{ item.EmployeeName }}</td>
          <td class="text-align-left">{{ item.GenderName }}</td>
          <td class="text-align-left">{{ formatDateData(item.DateOfBirth) }}</td>
          <td class="text-align-left">{{ item.IdentityNumber }}</td>
          <td class="text-align-left">{{ item.EmployeePosition }}</td>
          <td class="text-align-left">{{ item.DepartmentName }}</td>
          <td class="text-align-left">{{ item.BankAccountNumber }}</td>
          <td class="text-align-left">{{ item.BankName }}</td>
          <td class="text-align-left">{{ item.BankBranchName }}</td>
          <td class="text-align-center show-contexMenu" style="z-index: 2">
            <div class="function-col">
              <div class="function-col__update">
                <button class="btn-edit-epl" data-id="${employeeCode}" @click="openDetailFormEmployee(item)">
                  Sửa
                </button>
              </div>
              <div class="function-col__menu m-ml-8">
                <button>
                  <div class="m-icon-16 m-icon-arrow-down-blue"></div>
                </button>
                <div class="child-multi-choices" style="min-width: 120px">
                  <div class="duplication m-chil-dd" @click="duplicateEmployee(item)">Nhân bản</div>
                  <div class="delete-epl m-chil-dd" @click="confirmDeleteEmployee(item)">Xóa</div>
                </div>
              </div>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
import formMode from '@/enums/formMode';
import { mapActions, mapState } from 'vuex';
import ProgressLoading from "../../views/ProgressLoading.vue"
import Gender from "../../enums/gender"
import Button from "../../components/base/BaseButton.vue";

export default {
  name: "BaseTable",
  components:{
    ProgressLoading,
    Button
  },
  created() {
    this.getEmployees();
  },
  computed: mapState({
    employees:(state)=>state.employees,
    employee:(state)=>state.employee,
    
  }),
  methods: {
    ...mapActions(["getEmployees"]),
    ...mapActions(["setDetailEmployee"]),
    ...mapActions(["toggleForm"]),
    ...mapActions(["setTitleForm"]),
    ...mapActions(["setFormMode"]),
    ...mapActions(["toggleAlert"]),   
    ...mapActions(["setAlert"]), 
    ...mapActions(["toggleProgressLoading"]),
    ...mapActions(["getNewEmployeeCode"]),

    /**
     * hàm radom dữ liệu
     * Author:DungNP;
     */
    myFunction () {		
     this.randomNumber = Math.random()*100; //multiply to generate random number between 0, 100
    },


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

    searchEmployee(){
            const me = this;
            me.setFilter({
                pageSize: me.filter.pageSize,
                pageNumber: 1,
                employeeFilter: me.filter.employeeFilter
            });
            me.getEmployees();
        },

        refreshData(){
      const me = this;
      me.toggleProgressLoading();
      setTimeout(() => {
        me.getEmployees();
        me.toggleProgressLoading();
      }, 1000);
    },
    /**
     * Hàm định dạng dữ liệu ngày tháng về dạng dd/mm/yy
     * @param value Ngày tháng nhập vào
     * Author: DungNP
     */
    formatDateData(value) {
      if (value) {
        value = new Date(value);
        let date = value.getDate();
        date = date < 10 ? `0${date}` : date;
        let month = value.getMonth() + 1;
        month = month < 10 ? `0${month}` : month;
        let year = value.getFullYear();
        value = `${date}/${month}/${year}`;
        return value;
      }
    },
    /**
     * Mở form chi tiết nhân viên
     * @param item nhân viên được chọn
     * Author: DungNP 
     */
    openDetailFormEmployee(item){
        const me = this;
        me.setDetailEmployee(item);
        me.setTitleForm("Thông tin chi tiết nhân viên");
        me.toggleForm();
        me.setFormMode(formMode.update);
    },
    /**
     * Hàm xác nhận xóa nhân viên được chọn
     * @param employee nhân viên muốn xóa
     * Author: DungNP
     */
    confirmDeleteEmployee(employee){
      const me = this;
      me.setTitleForm("Xoá nhân viên");
      me.alert = {
          type:"confirmDelete",
          message:`Bạn có thực sự muốn xóa nhân viên <${employee.EmployeeCode}> không?`
      } 
      me.setDetailEmployee(employee);
      me.setAlert(me.alert);
      me.toggleAlert();
    },
        /**
     * Mở form thêm mới nhân viên để nhân bản
     * @param employee nhân viên được nhân bản
     * Author: Dung(21/11/2022)
     */
    duplicateEmployee(employee){
        const me = this;
        me.getNewEmployeeCode(employee);
        me.setDetailEmployee(employee);
        me.setTitleForm("Nhân bản nhân viên");
        me.toggleForm();
        me.setFormMode(formMode.insert);
    },

    DeleteList(){
      
      try {
        this.EmployeeCode=[];
        // if (!this.EmployeeCode.includes(employee)) {
        //   this.EmployeeCode.push(employee)
        // } else {
        //   this.EmployeeCode.splice(this.EmployeeCode.indexOf(employee), 1)
        // }
        var el= this.$refs.checkbox;
        console.log(el);
        for(const item of el){
          if(item.attributeCheck["checked"].value == "true"){
            this.EmployeeCode.push(item.attributeCheck["dataobj"].value);
          }
        }
        this.setTitleForm("Xoá nhân viên");
        
      } catch (error) {
        console.log(error);
      }
    }
  },
  data() {
    return {
      randomNumber:'',
      alert:{
        type:"",
        message:""
      }
    };
  }
};


</script>
<style scoped>
  @import url("../../css/layout/body.css");
  @import url("../../css/components/table.css");
</style>