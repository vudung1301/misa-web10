<template>
  <div class="m-popup">
    <div class="m-popup-content">
      <div class="popup-header">
        <div class="popup-title">
          <div class="popup-title-name">{{ titleForm }}</div>
        </div>
        <div class="popup-close">
          <div class="m-icon-24 m-icon-help" style="margin-right: 8px"></div>
          <div
            class="m-icon-24 m-icon-close m-close-add-popup js-close-form"
            title="Đóng (ESC)"
            @click="toggleForm"
          ></div>
        </div>
      </div>
      <div class="popup-content">
        <form action="" id="m-form-employee">
          <div class="m-form-content-4-col">
            <div class="m-col-1">
              <div class="m-layout-form-40-60 display-f">
                <div class="m-input-form-40 m-mb-24 m-mr-12">
                  <div class="display-f m-mb-8 " >
                    <div class="m-input-title-required m-input-title">Mã</div>
                    <div class="m-icon-require">&nbsp;*</div>
                  </div>
                  <input
                    type="text"
                    ref="focusMe" 
                    class="m-input-form required"
                    v-model="employee.EmployeeCode"
                    :class="{ 'm-input-form-error': errorInput.employeeCode }"
                    maxlength="25"
                    
                    tabindex="3"
                    @blur="validateEmployeeCode()"
                  />
                  <div class="err-message err-id-employee">
                    {{ errorInput.employeeCode }}
                  </div>
                </div>
                <div class="m-input-form-60 m-mb-24">
                  <div class="display-f m-mb-8">
                    <div class="m-input-title-required m-input-title">Tên</div>
                    <div class="m-icon-require">&nbsp;*</div>
                  </div>
                  <input
                    type="text"
                    class="m-input-form required"
                    v-model="employee.EmployeeName"
                    :class="{ 'm-input-form-error': errorInput.employeeName }"
                    maxlength="128"

                    tabindex="4"
                    @blur="validateEmployeeName()"
                  />
                  <div class="err-message">{{ errorInput.employeeName }}</div>
                </div>
              </div>
            </div>
            <div class="m-col-2">
              <div class="m-layout-form-40-60 display-f">
                <div class="m-pr-6 m-mb-24">
                  <div class="m-mb-8">
                    <div class="m-input-title">Ngày sinh</div>
                  </div>
                  <input
                    type="date"
                    class="m-input-form"
                    v-model="employee.DateOfBirth"
                    :class="{ 'm-input-form-error': errorInput.dateOfBirth }"
                    id="dateOB"
                    propName="DateOfBirth"
                    tabindex="5"
                    @blur="validateBirthDate()"
                  />
                  <div class="err-message">{{ errorInput.dateOfBirth }}</div>
                </div>
                <div class="m-input-form-60 m-mb-24">
                  <div class="m-mb-8">
                    <div class="m-input-title">Giới tính</div>
                  </div>
                  <div class="m-radio-group">
                    <label class="m-con-radio">
                      <input
                        type="radio"
                        class="m-input-radio"
                        name="Gender"
                        v-model="employee.Gender"
                        value="1"
                        id="rdMale"
                        tabindex="6"
                      />
                      <span class="m-radio-label">Nam</span>
                    </label>
                    <label class="m-con-radio">
                      <input
                        type="radio"
                        class="m-input-radio"
                        id="rdFemale"
                        name="Gender"
                        value="0"
                        v-model="employee.Gender"
                        tabindex="6"
                      />
                      <span class="m-radio-label">Nữ</span>
                    </label>
                    <label class="m-con-radio">
                      <input
                        type="radio"
                        class="m-input-radio"
                        name="Gender"
                        value="2"
                        v-model="employee.Gender"
                        id="rdOther"
                        tabindex="6"
                      />
                      <span class="m-radio-label">Khác</span>
                    </label>
                  </div>
                </div>
              </div>value
            </div>
          </div>
          <div class="m-form-conten-2-col">
            <div class="m-col-1">
              <div class="m-input-form-100 m-mb-24">
                <div class="display-f m-mb-8">
                  <div class="m-input-title-required m-input-title">Đơn vị</div>
                  <div class="m-icon-require">&nbsp;*</div>
                </div>

                <div class="dropdownlist dropdown-department">
                  <input
                    class="input dropdownlist__input required"
                    name="DepartmentName"
                    tabindex="7"
                    readonly="true"
                    id="departmentName"
                    propName="DepartmentName"
                    v-model="employee.DepartmentName"
                    :class="{ 'm-input-form-error': errorInput.departmentId }"
                    type="text"
                    @blur="validateDepartment()"
                  />

                  <div
                    class="dropdownlist__button btn-department"
                    id="btn-"
                    tabindex="8"
                    @click="toggleList"
                  >
                    <div
                      class="m-icon-arrow-dropdown m-icon-16"
                      style="margin: 0 auto; align-self: center"
                    ></div>
                  </div>
                  <div
                    class="dropdownlist__data top-100 dropdown-department"
                    v-if="isShowList"
                  >
                    <div v-for="(item, index) in departments" :key="index">
                      <div class="data-item" @click="chooseDepartment(item)">
                        {{ item.DepartmentName }}
                      </div>
                    </div>
                  </div>
                </div>
                <div class="err-message err-department-not-null">
                  {{ errorInput.departmentId }}
                </div>
              </div>
              <div class="m-input-form-100 m-mb-24">
                <div class="m-mb-8">
                  <div class="m-input-title">Chức danh</div>
                </div>
                <input
                  type="text"
                  class="m-input-form"
                  name="EmployeePosition"
                  id="position"
                  maxlength="128"
                  v-model="employee.EmployeePosition"
                  propName="EmployeePosition"
                  tabindex="11"
                />
                <div class="err-message"></div>
              </div>
            </div>
            <div class="m-col-2">
              <div class="m-layout-form-40-60 display-f">
                <div class="m-input-form-60 m-mr-12 m-mb-24">
                  <div class="m-mb-8">
                    <div class="m-input-title" title="Số chứng minh nhân dân">
                      Số CMND
                    </div>
                  </div>
                  <input
                    type="text"
                    class="m-input-form"
                    name="IdentityNumber"
                    id="identityNumber"
                    v-model="employee.IdentityNumber"
                    maxlength="20"
                    propName="IdentityNumber"
                    tabindex="9"
                    
                  />
                  <div class="err-message"></div>
                </div>
                <div class="m-input-form-40 m-mb-24">
                  <div class="m-mb-8">
                    <div class="m-input-title">Ngày cấp</div>
                  </div>
                  <input
                    type="date"
                    class="m-input-form"
                    name="IdentityDate"
                    id="identityDate"
                    v-model="employee.IdentityDate"
                    :class="{ 'm-input-form-error': errorInput.identityDate }"
                    propName="IdentityDate"
                    tabindex="10"
                    @blur="validateDateTimeProvided()"
                  />
                  <div class="err-message">{{ errorInput.identityDate }}</div>
                </div>
              </div>
              <div class="m-input-form-100 m-mb-24">
                <div class="m-mb-8">
                  <div class="m-input-title">Nơi cấp</div>
                </div>
                <input
                  type="text"
                  class="m-input-form"
                  name="IdentityPlace"
                  id="identityPlace"
                  propName="IdentityPlace"
                  v-model="employee.IdentityPlace"
                  tabindex="12"
                  
                />
                <div class="err-message"></div>
              </div>
            </div>
          </div>
          <div class="m-form-content-1-col display-f">
            <div class="m-input-form-100 m-mb-24">
              <div class="m-mb-8">
                <div class="m-input-title">Địa chỉ</div>
              </div>
              <input
                type="text"
                class="m-input-form m-input-form-100"
                name="Address"
                propName="Address"
                v-model="employee.Address"
                id="address"
                tabindex="13"
              />
              <div class="err-message"></div>
            </div>
          </div>

          <div class="m-form-content-3-col m-mb-24">
            <div class="m-input-form-33 m-mr-12">
              <div class="m-mb-8">
                <div class="m-input-title" title="Điện thoại di động">
                  ĐT di động
                </div>
              </div>
              <input
                type="text"
                class="m-input-form"
                name="TelephoneNumber"
                v-model="employee.TelephoneNumber"
                id="telephoneNumber"
                propName="TelephoneNumber"
                tabindex="14"
              />
              <div class="err-message"></div>
            </div>

            <div class="m-input-form-33 m-mr-12">
              <div class="m-mb-8">
                <div class="m-input-title" title="Điện thoại cố định">
                  ĐT cố định
                </div>
              </div>
              <input
                type="text"
                class="m-input-form"
                name="PhoneNumber"
                v-model="employee.PhoneNumber"
                id="phoneNumber"
                propName="PhoneNumber"
                tabindex="15"
              />
              <div class="err-message"></div>
            </div>
            <div class="m-input-form-33 m-mr-12">
              <div class="m-mb-8">
                <div class="m-input-title">Email</div>
              </div>
              <input
                type="text"
                id="emailEmployee"
                class="m-input-form"
                name="Email"
                v-model="employee.Email"
                :class="{ 'm-input-form-error': errorInput.emailEmployee }"
                propName="Email"
                tabindex="16"
                @blur="validateEmail(value)"
              />
              <div class="err-message">{{ errorInput.emailEmployee }}</div>
            </div>
          </div>

          <div class="m-form-content-3-col m-mb-24">
            <div class="m-input-form-33 m-mr-12">
              <div class="m-mb-8">
                <div class="m-input-title">Tài khoản ngân hàng</div>
              </div>
              <input
                type="text"
                class="m-input-form"
                name="BankAccountNumber"
                id="bankAccount"
                v-model="employee.BankAccountNumber"
                propName="BankAccountNumber"
                tabindex="17"
              />
              <div class="err-message"></div>
            </div>
            <div class="m-input-form-33 m-mr-12">
              <div class="m-mb-8">
                <div class="m-input-title">Tên ngân hàng</div>
              </div>
              <input
                type="text"
                class="m-input-form"
                id="bankName"
                v-model="employee.BankName"
                name="BankName"
                propName="BankName"
                tabindex="18"
              />
              <div class="err-message"></div>
            </div>
            <div class="m-input-form-33 m-mr-12">
              <div class="m-mb-8">
                <div class="m-input-title">Chi nhánh</div>
              </div>
              <input
                type="text"
                class="m-input-form"
                id="bankBranch"
                v-model="employee.BankBranchName"
                name="BankBranchName"
                propName="BankBranchName"
                tabindex="19"
              />
              <div class="err-message"></div>
            </div>
          </div>

          <div class="m-form-footer">
            <div class="m-form-part-cancel">
              <Button
                btnText="Hủy"
                addClass="m-btn"
                @click="toggleForm"
                tabindex="22"
              ></Button>
            </div>
            <div class="m-form-part-submit">
              <div class="m-form-part-save">
                <Button
                  btnText="Cất"
                  addClass="m-btn"
                  tabindex="21"
                  @click.prevent="save"
                ></Button>
              </div>
              <Button
                btnText="Cất và thêm"
                tabindex="20"
                @click.prevent="saveAndReset"
              ></Button>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions, mapState } from "vuex";
import Button from "../components/base/BaseButton.vue";
import axios from "axios";
import formMode from "@/enums/formMode";
import Gender from "../enums/gender";

// const me = this;
export default {
  name: "PopupDetailEmployee",
  components: {
    Button,
  },
  created() {
    this.getDepartments();
  },
  mounted() {
    //focus vào ô Mã nhân viên
    this.$refs.focusMe.focus()
  },
  data() {
    return {
      isShowList: false,
        errorInput: {
        employeeCode: "",
        employeeName: "",
        departmentId: "",
        dateOfBirth: "",
        emailEmployee: "",
        identityDate: "",
      },
      department: {
        departmentId: "",
        departmentName: "",
      },
      alert: {
        type: "",
        message: "",
      },
    };
  },
  
  computed: mapState({
    isShowForm: (state) => state.isShowForm,
    departments: (state) => state.departments,
    titleForm: (state) => state.titleForm,
    employee: (state) => state.employee,
    formMode: (state) => state.formMode,
    alert: (state) => state.alert,
  }),
  methods: {
    ...mapActions(["toggleForm"]),
    ...mapActions(["toggleProgressLoading"]),
    ...mapActions(["toggleNoticeMessage"]),
    ...mapActions(["toggleAlert"]),
    ...mapActions(["getEmployees"]),
    ...mapActions(["getDepartments"]),
    ...mapActions(["setTitleNotice"]),
    ...mapActions(["setDetailEmployee"]),
    ...mapActions(["setFormMode"]),
    ...mapActions(["setAlert"]),

    /**
     * Hàm đóng, mở danh sách đơn vị
     * Author: DungNP
     */
    toggleList() {
      this.isShowList = !this.isShowList;
    },

    /**
     * Hàm chọn tên đơn vị trong dropdownlist
     * @param {departmentIsChoose} phòng ban được chọn
     * Author: DungNP
     */
    chooseDepartment(departmentIsChoosed) {
      const me = this;
      me.employee.DepartmentId = departmentIsChoosed.DepartmentId;
      me.employee.DepartmentName = departmentIsChoosed.DepartmentName;
      me.isShowList = false;
    },

    /**
     * Hàm validate mã nhân viên
     * Author:DungNP
     */
    validateEmployeeCode() {
      const me = this;
      //Validate mã nhân viên
       let regex = /(NV)(\d+)$|(nv)(\d+)$/;
      if (!me.employee.EmployeeCode)
        me.errorInput.employeeCode = "Mã không để trống";
      else if (!regex.exec(me.employee.EmployeeCode))
         me.errorInput.employeeCode = "Mã phải là NV + số";
      else 
        me.errorInput.employeeCode ="";
    },
    
    /**
     * Hàm validate Tên nhân viên
     * Author:DungNP
     */
    validateEmployeeName() {
      const me = this;
      //Validate Tên nhân viên
      if (!me.employee.EmployeeName)
        me.errorInput.employeeName = "Tên  không để trống";
      else 
        me.errorInput.employeeName ="";
    },

    /**
     * Hàm validate các trường kiểu date
     * @param: value
     * Author: DungNP
     */
    validateDateTime(value, titleInput) {
      let dateTime = new Date(value);
      let errorMessage = "";
      if (dateTime > new Date()) {
        errorMessage = titleInput + " không lớn hơn ngày hiện tại";
      }
      return errorMessage;
    },
    /**
     * Hàm validate email đúng định dạng
     * Author: DungNP
     */
    validateEmail(value) {
      //Dòng code bên dưới (dòng 518) không phải là comment, phải có dòng này thì regexEmail mới dùng được, bỏ đi sẽ xuất hiện 2 lỗi
      /* eslint-disable no-useless-escape */
      let errorMessage = "";
      let regexEmail = /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/;
      if (!regexEmail.exec(value)) errorMessage = "Email sai định dạng";
      if (!value) errorMessage = "";
      return errorMessage;
    },
        /**
     * Hàm validate Phòng ban đúng định dạng
     * Author: DungNP
     */
    validateDepartment(){
      const me=this;
      //Validate phòng ban
      if (!me.employee.DepartmentId) {
        me.errorInput.departmentId = "Đơn vị không để trống";
      }
      else{
        me.errorInput.departmentId = "";
      }
    },
    validateBirthDate(){
      const me= this;
      me.errorInput.dateOfBirth = this.validateDateTime(
        me.employee.DateOfBirth,
        "Ngày sinh"
      );
    },
    validateDateTimeProvided(){
      const me= this;
      me.errorInput.identityDate = this.validateDateTime(
        me.employee.IdentityDate,
        "Ngày cấp"
      );
    },
   

    /**
     * Hàm validate dữ liệu nhâp vào form
     * Author: DungNP
     */
    validateForm() {
      const me = this;
      me.errorInput = {
        employeeCode: "",
        employeeName: "",
        departmentId: "",
        dateOfBirth: "",
        emailEmployee: "",
        identityDate: "",
      };
      //Validate Mã nhân viên
      this.validateEmployeeCode();

      //Validate Tên nhân viên
      this.validateEmployeeName();
      
      //Validate đơn vị
      this.validateDepartment();



      //Validate ngày sinh
      this.validateBirthDate();
      //Validate ngày cấp
      this.validateDateTimeProvided();
      //Validate Email phải đúng định dạng
      me.errorInput.emailEmployee = this.validateEmail(me.employee.Email);
      if (
        me.errorInput.employeeCode ||
        me.errorInput.employeeName ||
        me.errorInput.departmentId ||
        me.errorInput.dateOfBirth ||
        me.errorInput.identityDate ||
        me.errorInput.emailEmployee
      )
        return false;
      //Nếu validate đúng hết thì trả về true để phục vụ quá trình thêm mới
      return true;
    },

    /**
     * Hàm kích nút "Cất" để thêm mới hoặc sửa và đóng form sau khi xong
     * Author: DungNP
     */
    save() {
      // trước khi lưu thì validate dữ liệu
      const me = this;
      let isValid = me.validateForm();
      if (isValid) {
        if (me.formMode == formMode.insert) 
          me.insertEmployee(me.employee);
        else if (me.formMode == formMode.update) {
          me.updateEmployee(me.employee);
        }
      }
    },

    /**
     * Hàm kích nút "Cất và thêm" để thêm mới hoặc sửa và reset form để tiếp tục thêm mới
     * Author: DungNP
     */
    saveAndReset() {
      const me = this;
      let isValid = me.validateForm();
      if (me.formMode == formMode.update) 
        me.setFormMode(formMode.updateAndAdd);
      else me.setFormMode(formMode.insertAndAdd);
      if (isValid) {
        if (me.formMode == formMode.insertAndAdd) {
          me.insertEmployee(me.employee);
        } else if (me.formMode == formMode.updateAndAdd) {
          me.updateEmployee(me.employee);
        }
      }
    },

    /**
     * Hàm thêm mới nhân viên
     * @param {object} employee nhân viên được thêm mới
     * Author: DungNP
     */
    insertEmployee(employee) {
      try {
        const me = this;
        axios
          .post("https://amis.manhnv.net/api/v1/Employees", employee)
          .then(() => {
            me.toggleProgressLoading();
            //Load lại dữ liệu
            me.getEmployees();
            if (me.formMode == formMode.insert) {
              //Đóng form
              me.toggleForm();
              //Bật nút progress, tự tắt sau 0.5s
              setTimeout(() => {
                me.toggleProgressLoading();
                //Bật thông báo thêm thành công
                me.toggleNoticeMessage();
              }, 500);
            } else if (me.formMode == formMode.insertAndAdd) {
              setTimeout(() => {
                me.toggleProgressLoading();
                //Bật thông báo thêm thành công
                me.toggleNoticeMessage();
              }, 500);
              me.setDetailEmployee({ Gender: Gender.male });
            }
            me.setTitleNotice("Thêm nhân viên thành công");
            setTimeout(() => {
              me.toggleNoticeMessage();
            }, 5000);
          })
          .catch(() => {
            let empcode = me.employee.EmployeeCode;
            me.alert = {
              type: "danger",
              message: `Mã nhân viên <${empcode}> tồn tại trong hệ thống, vui lòng kiểm tra lại`,
              // message: error.response.data.userMsg
            };
            me.setAlert(me.alert);
            me.toggleAlert();
          });
      } catch (error) {
        console.log(error);
      }
    },
  /**
   * Hàm lấy mã nhân viên mới phục vụ chức năng thêm mới nhân viên
   *t 
   * Author: DungNp (8/11/2022)
   */
   getNewEmployeeCode(employee){
    try {
      axios.get('https://amis.manhnv.net/api/v1/Employees/NewEmployeeCode')
      .then(response => employee.commit('getNewEmployeeCode', response.data))
    } catch (error) {
      console.log(error);
    }
  },
    /**
     * Hàm cập nhật nhân viên
     * @param {object} employee nhân viên được cập nhật
     * Author: DungNP
     */
    updateEmployee(employee) {
      try {
        const me = this;
        axios
          .put(
            `https://amis.manhnv.net/api/v1/Employees/${employee.EmployeeId}`,
            employee
          )
          .then(() => {
            me.toggleProgressLoading();
            //Load lại dữ liệu
            me.getEmployees();
            if (me.formMode == formMode.update) {
              //Đóng form
              me.toggleForm();
              //Bật nút progress, tự tắt sau 0.5s
              setTimeout(() => {
                me.toggleProgressLoading();
                //Bật thông báo sửa thành công
                me.toggleNoticeMessage();
              }, 500);
            } else if (me.formMode == formMode.updateAndAdd) {
              setTimeout(() => {
                me.toggleProgressLoading();
                me.toggleNoticeMessage();
              }, 500);
              me.setDetailEmployee({ Gender: Gender.male });
            }
            me.setTitleNotice("Sửa nhân viên thành công");
            setTimeout(() => {
              me.toggleNoticeMessage();
            }, 5000);
          })
          .catch(() => {
            me.alert = {
              type: "danger",
              message: `Mã nhân viên <${me.employee.EmployeeCode}> tồn tại trong hệ thống, vui lòng kiểm tra lại`,
            };
            me.setAlert(me.alert);
            me.toggleAlert();
          });
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style scoped>
@import url("../css/components/popup.css");
</style>