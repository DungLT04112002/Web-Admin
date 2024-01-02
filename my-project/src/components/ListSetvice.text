<template>
  <div class="frame-service">
    <div class="service-list">
      <div class="background-image"></div>
      <div class="content">
        <div class="service-container">
          <div
            v-for="appointment in appointments"
            :key="appointment.MAKH"
            class="service-card"
          >
            <div class="service-details">
              <h2>{{ appointment.TENDV }}</h2>
              <p>Ngày Khám: {{ formatNgayKham(appointment.NGAYKhAM) }}</p>
              <p>Giá Tiền: {{ appointment.GIA }} VND</p>
              <div v-if="appointment.showDetails" class="service-details">
                <h2>Thông Tin Dịch Vụ</h2>
                <p>
                  <strong>Ngày Khám:</strong>
                  {{ formatNgayKham(appointment.NGAYKhAM) }}
                </p>
                <p><strong>Giá Tiền:</strong> {{ appointment.GIA }} VND</p>
                <p><strong>Bác Sĩ:</strong> {{ appointment.TENNS }}</p>
                <p><strong>Dịch Vụ:</strong> {{ appointment.TENDV }}</p>
                <p><strong>Lời Nhắn:</strong> {{ appointment.GHICHU }}</p>
              </div>

              <div class="buttons">
                <button class="btn-details" @click="viewDetails(appointment)">
                  Xem Chi Tiết
                </button>
                <button class="btn-pay" @click="generateQRCode(appointment)">
                  Thanh Toán
                </button>
                <button
                  class="btn-delete"
                  @click="
                    deleteAppointment(
                      appointment.MAKH,
                      formatNgayKham(appointment.NGAYKhAM)
                    )
                  "
                >
                  Xóa
                </button>
                <button class="btn-edit" @click="openEditForm(appointment)">
                  Chỉnh Sửa
                </button>
              </div>
            </div>
          </div>
        </div>
        <div class="payment-voucher">
          <div class="background-payment">
            <p class="text-payment">QR CODE</p>
            <div v-if="selectedAppointment" class="payment-details">
              <img v-if="qrCode" :src="qrCode" alt="QR Code" class="qr-code" />
              <h2>Thanh Toán cho: {{ selectedAppointment.TENDV }}</h2>
            </div>
          </div>
          <div class="voucher">
            <p class="text-payment">NHẬP MÃ GIẢM GIÁ</p>
            <input type="text" id="" />
          </div>
        </div>
      </div>
    </div>
    <div v-if="editFormVisible" class="edit-form-modal">
      <div class="edit-form-content">
        <table>
          <tr>
            <td><label for="editedTime">Giờ Khám:</label></td>
            <td><input type="text" v-model="editedTime" id="editedTime" /></td>
          </tr>
          <tr>
            <td><label for="editedReminder">Lời Nhắc Hẹn:</label></td>
            <td>
              <textarea v-model="editedReminder" id="editedReminder"></textarea>
            </td>
          </tr>
        </table>

        <!-- Nút lưu chỉnh sửa -->
        <button @click="saveEdit">Lưu Chỉnh Sửa</button>
        <!-- Nút đóng modal -->
        <button @click="closeEditForm" class="close-btn">Đóng</button>
      </div>
    </div>
  </div>
</template>

<script>
import QRCode from "qrcode";

export default {
  data() {
    return {
      appointments: [],
      selectedAppointment: null,
      qrCode: null,
      editFormVisible: false,
      editedTime: "",
      editedReminder: "",
      editedAppointment: null,
    };
  },
  created() {
    this.fetchAppointments();
  },
  methods: {
    async fetchAppointments() {
      try {
        const response = await fetch("http://localhost:3000/appointments");
        const data = await response.json();
        this.appointments = data.map((appointment) => ({
          ...appointment,
          showDetails: false,
        }));
      } catch (error) {
        console.error("Lỗi khi lấy dữ liệu:", error);
      }
    },
    viewDetails(appointment) {
      appointment.showDetails = !appointment.showDetails;
    },

    generateQRCode(appointment) {
      this.selectedAppointment = appointment;
      const content = `2|99|0339997866|LUU TIEN DUNG|1|0|0|${appointment.GIA}|MA KHACH HANG|transfer_myqr`;
      QRCode.toDataURL(content)
        .then((url) => {
          this.qrCode = url;
        })
        .catch((err) => {
          console.error(err);
          alert("Lỗi khi tạo mã QR");
        });
    },
    async deleteAppointment(MAKH, NGAYKhAM) {
      try {
        const response = await fetch(
          `http://localhost:3000/appointments/${MAKH}/${NGAYKhAM}`,
          {
            method: "DELETE",
          }
        );

        const data = await response.json();

        if (response.ok) {
          console.log(data.message);
          // Nếu muốn cập nhật lại danh sách sau khi xóa, gọi lại fetchAppointments
          this.fetchAppointments();
        } else {
          console.error(data.error);
        }
      } catch (error) {
        console.error("Lỗi khi gọi API xóa:", error);
      }
    },
    formatNgayKham(dateString) {
      const date = new Date(dateString);
      const year = date.getFullYear();
      const month = (date.getMonth() + 1).toString().padStart(2, "0");
      const day = date.getDate().toString().padStart(2, "0");
      return `${year}-${month}-${day}`;
    },
    openEditForm(appointment) {
      // Hiển thị modal và điền thông tin cần chỉnh sửa
      this.editFormVisible = true;
      this.editedTime = appointment.KHUNGGIO;
      this.editedReminder = appointment.GHICHU;
      this.editedAppointment = appointment;
      this.editedAppointment.NGAYKhAM = this.formatNgayKham(
        this.editedAppointment.NGAYKhAM
      );
    },

    closeEditForm() {
      // Đóng modal và làm sạch thông tin chỉnh sửa
      this.editFormVisible = false;
      this.editedTime = "";
      this.editedReminder = "";
      this.editedAppointment = null;
    },

    async saveEdit() {
      try {
        // Gọi API chỉnh sửa thông tin lịch khám với thông tin đã chỉnh sửa
        const response = await fetch(
          `http://localhost:3000/appointments/${this.editedAppointment.MAKH}/${this.editedAppointment.NGAYKhAM}`,
          {
            method: "PUT",
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify({
              KHUNGGIO: this.editedTime,
              GHICHU: this.editedReminder,
            }),
          }
        );

        const data = await response.json();

        if (response.ok) {
          console.log(data.message);
          // Đóng modal sau khi lưu chỉnh sửa và cập nhật danh sách
          this.closeEditForm();
          this.fetchAppointments();
        } else {
          console.error(data.error);
        }
      } catch (error) {
        console.error("Lỗi khi gọi API chỉnh sửa:", error);
      }
    },
  },
};
</script>

<style scoped>
/* CSS cho div chứa toàn bộ nội dung */
.service-container {
  margin: 20px;
  width: 1400px;
  float: left;
}
.payment-voucher {
  float: right;
  width: 400px;
  margin: 20px;
  padding: 10px;
  background-color: white;
  border-radius: 15px;
}
.background-payment {
  float: top;
  background-color: gray;
  border-radius: 10px;
  box-shadow: 2px 2px 2px;
  padding: 10px;
  margin: 10px;
}
.voucher {
  float: top;
  background-color: gray;
  border-radius: 10px;
  box-shadow: 2px 2px 2px;
  padding: 10px;
  margin: 10px;
}

.frame-service {
  background-color: gray;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
}

/* CSS cho danh sách dịch vụ */
.service-list {
  background-color: brown;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.2);
  border-radius: 10px;
  padding: 20px;
  max-width: 1900px;
  width: 100%;
}

/* CSS cho hình ảnh phía sau */
.background-image {
  background-color: #0072ff;
  height: 200px;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
}

/* CSS cho tiêu đề */
.content h1 {
  font-size: 24px;
  margin-top: 20px;
  text-align: center;
  color: #333;
}

/* CSS cho từng dịch vụ */
.service-card {
  display: flex;
  margin-bottom: 20px;
  background-color: #fff;
  box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.2);
  border-radius: 10px;
  overflow: hidden;
}

/* CSS cho hình ảnh dịch vụ */
.service-image img {
  width: 100px;
  height: 100px;
  object-fit: cover;
  border-radius: 0;
}

/* CSS cho chi tiết dịch vụ */
.service-details {
  flex: 1;
  padding: 20px;
}
.btn-pay {
  background-color: green;
}
.btn-delete {
  background-color: red;
}
.btn-details {
  background-color: blue;
}
/* CSS cho nút */
.buttons button {
  border: none;
  padding: 10px 20px;
  border-radius: 10px;
  cursor: pointer;
  margin-right: 10px;
  box-shadow: 2px 2px 2px #aaa;
}

/* CSS cho QR Code */
.qr-code {
  max-width: 100%;
  height: auto;
  display: block;
  margin: 10px auto;
}

/* CSS cho tiêu đề QR Code */
.text-payment {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 10px;
  text-align: center;
}

/* CSS cho chi tiết thanh toán */
.payment-details {
  text-align: center;
}

/* Thêm một chút động tác khi hover vào các nút */
.buttons button:hover {
  background-color: gray;
}
.edit-form-modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000; /* Ensure it's on top of other elements */
}

.edit-form-content {
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  width: 80%; /* Adjust the width as needed */
  max-width: 500px; /* Set a maximum width for larger screens */
}

/* CSS for the close button in the modal */
.edit-form-content button.close-btn {
  background-color: #333;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 10px;
}

.edit-form-content table {
  width: 100%;
  margin-bottom: 15px;
}

.edit-form-content td {
  padding: 10px;
}


</style>
