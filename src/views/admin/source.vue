<template>
  <div>
    <v-toolbar dense flat color="grey lighten-4">
      <v-toolbar-title class="pl-2">التدريسين</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn color="#55AB88" dark @click="rest()">اضافة تدريسي</v-btn>
    </v-toolbar>
    <v-container class="grey lighten-5">
      <v-row class="mx-1">
        <v-text-field
          v-model="search"
          outlined
          dense
          hide-details
          filled
          prepend-inner-icon="search"
          placeholder="اكتب للبحث"
        />
      </v-row>
      <v-row no-gutters>
        <v-col>
          <v-data-table
            :loading="loading"
            loading-text="جاري جلب المعلومات"
            class="mt-4"
            :headers="headers"
            :items="items"
            :page.sync="page"
            :items-per-page="itemsPerPage"
            hide-default-footer
            @page-count="pageCount = $event"
            :search="search"
          >
            <template v-slot:item.actions="{ item }">
              <v-tooltip bottom>
                <template v-slot:activator="{ on }">
                  <v-btn class="indigo--text" icon v-on="on" large>
                    <v-icon l @click="delet(item.id)" color="red">mdi-delete-outline</v-icon>
                  </v-btn>
                </template>
                <span>حذف</span>
              </v-tooltip>
              <v-tooltip bottom>
                <template v-slot:activator="{ on }">
                  <v-btn class="indigo--text" icon v-on="on" large>
                    <v-icon l @click="edit(item.id)" color="green">mdi-pencil-outline</v-icon>
                  </v-btn>
                </template>
                <span>تعديل</span>
              </v-tooltip>
            </template>
            <template v-slot:no-data>
              <h3>لاتوجد بيانات لعرضها</h3>
            </template>
          </v-data-table>
          <div class="text-center pt-2">
            <v-pagination v-model="page" :length="pageCount"></v-pagination>
          </div>
        </v-col>
      </v-row>
    </v-container>
    <v-dialog v-model="detailsDialog" width="400" persistent>
      <v-card>
        <v-toolbar dark color="primary">
          <v-toolbar-title>
            <h3>اضافة تدريسي</h3>
          </v-toolbar-title>
          <v-spacer />
          <v-btn @click="detailsDialog = false" icon>
            <v-icon>close</v-icon>
          </v-btn>
        </v-toolbar>
        <br />
        <v-container>
          <v-row>
            <v-col cols="12" sm="12" md="12">
              <v-text-field
                outlined
                label=" اسم الاستاذ : "
                v-model="name"
                :rules="[v => !!v || 'هذا الحقل مطلوب']"
              />
              <v-select v-model="value" :items="itemso" chips label="Chips" multiple outlined></v-select>

              <v-file-input
                v-model="file"
                label="اختار صورة"
                outlined
                :rules="[v => !!v || 'هذا الحقل مطلوب']"
              ></v-file-input>
            </v-col>
          </v-row>
          <br />
        </v-container>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="red darken-1" text @click="detailsDialog = false">اغلاق</v-btn>
          <v-btn color="blue darken-1" text @click="uploadImg()" :loading="loading2">حفظ</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="detailsDialog2" width="400" persistent>
      <v-card>
        <v-toolbar dark color="primary">
          <v-toolbar-title>
            <h3>تعديل التدريسي</h3>
          </v-toolbar-title>
          <v-spacer />
          <v-btn @click="detailsDialog2 = false" icon>
            <v-icon>close</v-icon>
          </v-btn>
        </v-toolbar>
        <br />
        <v-container>
          <v-row>
            <v-col cols="12" sm="12" md="12">
              <v-text-field
                outlined
                label=" اسم الاستاذ : "
                v-model="name"
                :rules="[v => !!v || 'هذا الحقل مطلوب']"
              />
              <v-select v-model="value" :items="itemso" chips label="Chips" multiple outlined></v-select>

              <v-file-input
                v-model="file"
                label="اختار صورة"
                outlined
                :rules="[v => !!v || 'هذا الحقل مطلوب']"
              ></v-file-input>
            </v-col>
          </v-row>
          <br />
        </v-container>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="red darken-1" text @click="detailsDialog2 = false">اغلاق</v-btn>
          <v-btn color="blue darken-1" text @click="uploadImg2()" :loading="loading2">حفظ</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import Swal from "sweetalert2";
import axios from "axios";

export default {
  data() {
    return {
      itemso: [
        "HCI",
        "computer system",
        "analysis systim",
        "java",
        "mobile apps",
        "projects",
        "managment",
        "security"
      ],
      value: ["1", "2", "3", "4", "5", "6", "7", "8"],
      details: "",
      file: null,
      img_path: "",
      name: "",
      phone: "",
      fbLink: "",
      twLink: "",
      teleLink: "",
      search: "",
      loading2: false,
      detailsDialog: false,
      detailsDialog2: false,
      page: 1,
      pageCount: 0,
      itemsPerPage: 10,
      headers: [
        { sortable: false },
        { text: "اسم الاستاذ ", value: "name" },
        { text: "المواد ", value: "moad" },
        { text: "العمليات", value: "actions" }
      ],

      coontractCount: [],
      loading: false,
      editId: "",
      items: [
        {
          name: "raghd hamid",
          moad: ["java", "mobile apps"]
        },
        {
          name: "mhmd ali",
          moad: ["system", "data", " apps"]
        },
        {
          name: "test test",
          moad: ["php", "java", "mobile apps"]
        }
      ]
    };
  },
  methods: {
    rest() {
      this.name = "";
      this.phone = "";
      this.details = "";
      this.file = "";
      this.detailsDialog = true;
    },
    uploadImg() {
      let formData = new FormData();
      formData.append("file", this.file);
      // console.log(formData);
      this.loading2 = true;

      axios
        .post("file/upload", formData, {
          headers: {
            "Content-Type": "multipart/form-data",
            access_token: `Bearer ${localStorage.getItem("tokin")}`
          }
        })
        .then(res => {
          //   console.log(res);
          this.img_path = res.data.url;
          this.addLap();
        })
        .catch(err => {
          Swal.fire("خطا في رفع الصورة");
          this.loading2 = false;
        });
    },
    uploadImg2() {
      if (this.file) {
        let formData = new FormData();
        formData.append("file", this.file);
        // console.log(formData);
        this.loading2 = true;

        axios
          .post("file/upload", formData, {
            headers: {
              "Content-Type": "multipart/form-data",
              access_token: `Bearer ${localStorage.getItem("tokin")}`
            }
          })
          .then(res => {
            //   console.log(res);
            this.img_path = res.data.url;
            this.editInfo();
          })
          .catch(err => {
            Swal.fire("خطا في رفع الصورة");
            this.loading2 = false;
          });
      } else {
        this.editInfo();
      }
    },
    editInfo() {
      if (this.name && this.phone && this.details) {
        this.loading2 = true;
        const cityData = {
          title: this.name,
          cover: this.img_path,
          details: this.details,
          links: [this.phone],
          id: this.editId
        };
        axios
          .put("tip", cityData, {
            headers: {
              access_token: `Bearer ${localStorage.getItem("tokin")}`
            }
          })
          .then(res => {
            this.rnData();
            this.loading2 = false;
            this.detailsDialog2 = false;
            Swal.fire({
              title: "تمت عملية التعديل بنجاح",
              icon: "success",
              confirmButtonText: "اغلاق"
            });
          })
          .catch(err => {
            this.loading2 = false;
            Swal.fire({
              title: "خطا في ارسال المعلومات",
              icon: "error",
              confirmButtonText: "اغلاق"
            });
          });
      } else {
        Swal.fire({
          title: "تاكد من ادخال المعلومات",
          icon: "error",
          confirmButtonText: "اغلاق"
        });
      }
    },
    addLap() {
      if (this.name && this.phone && this.img_path) {
        this.loading2 = true;
        const cityData = {
          name: this.name,
          logo: this.img_path,
          webUrl: this.phone,
          facebookUrl: this.fbLink,
          twitterUrl: this.twLink,
          telegramUrl: this.teleLink
        };
        axios
          .post("source", cityData, {
            headers: {
              access_token: `Bearer ${localStorage.getItem("tokin")}`
            }
          })
          .then(res => {
            this.rnData();
            this.loading2 = false;
            this.detailsDialog = false;
            Swal.fire({
              title: "تمت عملية الاضافة بنجاح",
              icon: "success",
              confirmButtonText: "اغلاق"
            });
          })
          .catch(err => {
            this.loading2 = false;
            Swal.fire({
              title: "خطا في ارسال المعلومات",
              icon: "error",
              confirmButtonText: "اغلاق"
            });
          });
      } else {
        Swal.fire({
          title: "تاكد من ادخال المعلومات",
          icon: "error",
          confirmButtonText: "اغلاق"
        });
      }
    },
    rnData() {
      this.loading = true;
      axios
        .get("source", {
          headers: {
            access_token: `Bearer ${localStorage.getItem("tokin")}`
          }
        })
        .then(res => {
          this.loading = false;
          this.items = res.data;
        })
        .catch(err => {
          this.loading = false;
        });
    },
    delet(id) {
      Swal.fire({
        title: "هل انت متاكد من حذف ؟",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "نعم",
        cancelButtonText: "لا"
      }).then(result => {
        if (result.value) {
          axios
            .delete("Lab/deleteLab?ID=" + id)
            .then(res => {
              this.rnData();
              Swal.fire({
                title: "تمت عملية الحذف بنجاح",
                icon: "success",
                confirmButtonText: "اغلاق"
              });
            })
            .catch(err => {
              Swal.fire({
                title: "فشلت عمليت الحذف",
                icon: "error",
                confirmButtonText: "اغلاق"
              });
            });
        }
      });
    },
    edit(id) {
      axios.get("tip/" + id).then(res => {
        this.name = res.data.title;
        this.phone = res.data.links;
        this.details = res.data.details;
        this.img_path = res.data.cover;
        this.editId = id;
        this.detailsDialog2 = true;
      });
    }
  },
  created() {
    // this.rnData();
  }
};
</script>
