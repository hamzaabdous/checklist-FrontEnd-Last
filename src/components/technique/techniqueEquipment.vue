<template>
  <div style="padding: 50px; padding-top: 2%">
    <v-row style="text-align: center">
      <v-col cols="12" md="12">
        <h3>
          Equipement :
          <span class="red--text">
            {{ this.EquipmentsByCounter.nameEquipment }}</span
          >
        </h3>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="4" md="4">
        <template>
          <v-card class="mx-auto" max-width="200" outlined>
            <v-list-item three-line>
              <v-list-item-content>
                <div class="text-overline mb-4 red--text">
                  Pending Damage Tickets
                </div>
                <v-list-item-title class="text-h5 mb-1 red--text">
                  {{ this.EquipmentsByCounter.damagedCount }}
                </v-list-item-title>
              </v-list-item-content>

              <v-list-item-avatar tile size="60" color="white">
                <v-icon color="red" large>
                  mdi-alarm-light
                </v-icon></v-list-item-avatar
              >
            </v-list-item>
          </v-card>
        </template>
      </v-col>
      <v-col cols="4" md="4">
        <template>
          <v-card class="mx-auto" max-width="200" outlined>
            <v-list-item three-line>
              <v-list-item-content>
                <div class="text-overline mb-4 deep-orange--text">
                  CONFIRMED DAMAGE TICKETS
                </div>
                <v-list-item-title class="text-h5 mb-1 deep-orange--text">
                  {{ this.EquipmentsByCounter.confirmedCount }}
                </v-list-item-title>
              </v-list-item-content>

              <v-list-item-avatar tile size="60" color="white">
                <v-icon color="deep-orange" large>
                  mdi-bell-check
                </v-icon></v-list-item-avatar
              >
            </v-list-item>
          </v-card>
        </template>
      </v-col>
      <v-col cols="4" md="4">
        <template>
          <v-card class="mx-auto" max-width="200" outlined>
            <v-list-item three-line>
              <v-list-item-content>
                <div class="text-overline mb-4 blue--text">
                  Closed Damage Tickets
                </div>
                <v-list-item-title class="text-h5 mb-1 blue--text">
                  {{ this.EquipmentsByCounter.closedCount }}
                </v-list-item-title>
              </v-list-item-content>

              <v-list-item-avatar tile size="60" color="white">
                <v-icon color="blue" large>
                  mdi-lock-open
                </v-icon></v-list-item-avatar
              >
            </v-list-item>
          </v-card>
        </template>
      </v-col>
    </v-row>
    <div style="padding: 3px; padding-top: 4%">
      <v-data-table
        :headers="headers"
        :items="damageByEquipments"
        :search="search"
        :loading="loading"
        sort-by="item.id"
        class="elevation-1"
      >
        <template v-slot:top>
          <v-dialog v-model="dialogimage" max-width="700px">
            <v-card>
              <v-toolbar dark color="primary">
                <v-btn icon dark @click="dialogimage = false">
                  <v-icon>mdi-close</v-icon>
                </v-btn>
                <v-toolbar-title>Settings</v-toolbar-title>
                <v-spacer></v-spacer>
                <v-toolbar-items> </v-toolbar-items>
              </v-toolbar>

              <v-card-title class="text-h5"> Description : </v-card-title>
              <v-col cols="12" md="12">
                <v-textarea
                  solo
                  name="input-7-4"
                  label="Description"
                  v-model="photo.description"
                ></v-textarea>
              </v-col>
              <v-card-title class="text-h5"> Add picture : </v-card-title>
              <v-col cols="12" md="12">
                <v-file-input
                  label="Pictures"
                  v-model="photo.photos"
                  filled
                  multiple
                  prepend-icon="mdi-camera"
                ></v-file-input>
              </v-col>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn depressed color="" @click="dialogimage = false">
                  Close
                </v-btn>
                <v-btn depressed color="primary" @click="sendImage()">
                  Save
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogresolve" max-width="700px">
            <v-card>
              <v-toolbar dark color="primary">
                <v-btn icon dark @click="dialogresolve = false">
                  <v-icon>mdi-close</v-icon>
                </v-btn>
                <v-toolbar-title>Settings</v-toolbar-title>
                <v-spacer></v-spacer>
                <v-toolbar-items> </v-toolbar-items>
              </v-toolbar>

              <v-card-title class="text-h5"> Description : </v-card-title>
              <v-col cols="12" md="12">
                <v-textarea
                  solo
                  name="input-7-4"
                  label="Description"
                  v-model="confirmDamage.resolveDescription"
                ></v-textarea>
              </v-col>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn depressed color="" @click="dialogresolve = false">
                  Close
                </v-btn>
                <v-btn depressed color="primary" @click="confirmed()">
                  Save
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogreverted" max-width="700px">
            <v-card>
              <v-toolbar dark color="primary">
                <v-btn icon dark @click="dialogreverted = false">
                  <v-icon>mdi-close</v-icon>
                </v-btn>
                <v-toolbar-title>Settings</v-toolbar-title>
                <v-spacer></v-spacer>
                <v-toolbar-items> </v-toolbar-items>
              </v-toolbar>

              <v-card-title class="text-h5"> Description : </v-card-title>
              <v-col cols="12" md="12">
                <v-textarea
                  solo
                  name="input-7-4"
                  label="Description"
                  v-model="revertDamage.revertedDescription"
                ></v-textarea>
              </v-col>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn depressed color="" @click="dialogreverted = false">
                  Close
                </v-btn>
                <v-btn depressed color="primary" @click="revert()">
                  Save
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="600px">
            <v-card>
              <v-toolbar dark color="error">
                <v-toolbar-title>Warning !</v-toolbar-title>
              </v-toolbar>
              <v-card-title class="text-h5"
                >Are you sure you want to delete this Damage ?</v-card-title
              >
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn depressed color="" @click="dialogDelete = false"
                  >Cancel</v-btn
                >
                <v-btn depressed color="error" @click="deleteDamage()"
                  >OK</v-btn
                >
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialog" max-width="1200px">
            <v-card>
              <v-toolbar dark color="primary">
                <v-btn icon dark @click="closedtailedialoge">
                  <v-icon>mdi-close</v-icon>
                </v-btn>
                <v-toolbar-title>Settings</v-toolbar-title>
                <v-spacer></v-spacer>
                <v-btn color="blue-grey" @click="dialogimage = true">
                  <v-icon color="white" medium> mdi-camera </v-icon>
                  Add pictures
                </v-btn>
              </v-toolbar>
              <v-card-title class="text-h5 blue--text text--darken-3">
                Damage Details:
              </v-card-title>
              <v-container>
                <v-row>
                  <v-col>
                    <table class="DamageDetails">
                      <tbody>
                        <tr>
                          <td><h3>DamageType</h3></td>
                          <td class="valueColumn">
                            <h4>{{ damageSelect.damage_type.name }}</h4>
                          </td>
                        </tr>
                        <tr>
                          <td><h3>Description</h3></td>
                          <td class="valueColumn">
                            <h5 v-if="damageSelect.description == null">
                              Empty
                            </h5>

                            <h4 v-else>{{ damageSelect.description }}</h4>
                          </td>
                        </tr>
                        <tr>
                          <td><h3>Status</h3></td>
                          <td class="valueColumn">
                            <h5 v-if="damageSelect.status == null">Empty</h5>

                            <h4 v-else>{{ damageSelect.status }}</h4>
                          </td>
                        </tr>
                        <tr>
                          <td><h3>Declared At</h3></td>
                          <td class="valueColumn">
                            <h5 v-if="damageSelect.declaredAt == null">
                              Empty
                            </h5>

                            <h4 v-else>{{ damageSelect.declaredAt }}</h4>
                          </td>
                        </tr>
                        <tr>
                          <td><h3>Closed At</h3></td>
                          <td class="valueColumn">
                            <h5 v-if="damageSelect.closedAt == null">Empty</h5>

                            <h4 v-else>{{ damageSelect.closedAt }}</h4>
                          </td>
                        </tr>
                        <tr>
                          <td><h3>Reverted Times</h3></td>
                          <td class="valueColumn">
                            <h5 v-if="damageSelect.revertedTimes == null">
                              Empty
                            </h5>

                            <h4 v-else>{{ damageSelect.revertedTimes }}</h4>
                          </td>
                        </tr>
                        <tr>
                          <td><h3>Updated at</h3></td>
                          <td class="valueColumn">
                            <h5 v-if="damageSelect.updated_at == null">
                              Empty
                            </h5>

                            <h4 v-else>{{ damageSelect.updated_at }}</h4>
                          </td>
                        </tr>
                        <tr>
                          <td><h3>Created at</h3></td>
                          <td class="valueColumn">
                            <h5 v-if="damageSelect.created_at == null">
                              Empty
                            </h5>

                            <h4 v-else>{{ damageSelect.created_at }}</h4>
                          </td>
                        </tr>
                      </tbody>
                    </table>
                  </v-col>
                </v-row>
              </v-container>
              <v-card-title class="text-h5 blue--text text--darken-3">
                Photos :
              </v-card-title>
              <v-container class="DamageDetails">
                <v-row class="photos">
                  <template>
                    <v-row v-if="damageSelect.photos.length == 0">
                      <v-col cols="4">
                        <h5>Empty</h5>
                      </v-col>
                    </v-row>
                    <v-row v-else>
                      <v-col
                        v-for="item in damageSelect.photos"
                        :key="item.id"
                        class="d-flex child-flex"
                        cols="4"
                      >
                        <v-img
                          max-height="150"
                          max-width="200"
                          :src="`http://localhost:8000/storage/cdn/damagePhotos/${item.filename}`"
                          aspect-ratio="1"
                          class="grey lighten-2 imageRadius"
                          @click="showImage(item)"
                        >
                          <template v-slot:placeholder>
                            <v-row
                              class="fill-height ma-0"
                              align="center"
                              justify="center"
                            >
                              <v-progress-circular
                                indeterminate
                                color="grey lighten-5"
                              ></v-progress-circular>
                            </v-row>
                          </template>
                        </v-img>
                      </v-col>
                    </v-row>
                  </template>
                </v-row>
              </v-container>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn depressed color="primary" @click="opendialogresolve">
                  Confirme damage
                </v-btn>
                <v-btn depressed color="red" @click="closed">
                  Close damage
                </v-btn>
                <v-btn depressed color="red" @click="opendialogreverted">
                  Revert damage
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog
            v-model="dialogimageShow"
            fullscreen
            hide-overlay
            transition="dialog-bottom-transition"
          >
            <v-toolbar dark color="primary">
              <v-btn icon dark @click="dialogimageShow = false">
                <v-icon>mdi-close</v-icon>
              </v-btn>
              <v-toolbar-title>Picture</v-toolbar-title>
              <v-spacer></v-spacer>
              <v-btn
                class="mr-2 white--text"
                color="red"
              >
                <v-icon medium class="mr-2">mdi-folder-image</v-icon>
                <a
                  :href="`http://localhost:8000/storage/cdn/damagePhotos/${PhotoShow.filename}`"
                  download
                  target="_blank"
                >
                  download picture</a
                >
              </v-btn>
            </v-toolbar>
            <v-card>
              <div class="p-100">
                <v-img
                  :lazy-src="`http://localhost:8000/storage/cdn/damagePhotos/${PhotoShow.filename}`"
                  max-height="90%"
                  max-width="100%"
                  :src="`http://localhost:8000/storage/cdn/damagePhotos/${PhotoShow.filename}`"
                ></v-img>
              </div>
            </v-card>
          </v-dialog>
          <v-toolbar flat>
            <v-text-field
              v-model="search"
              append-icon="mdi-magnify"
              label="Search"
              single-line
              hide-details
            ></v-text-field>
            <v-spacer></v-spacer>
          </v-toolbar>
        </template>
        <template v-slot:[`item.actions`]="{ item }">
          <v-btn
            color="teal"
            class="mr-2 btn white--text"
            @click="pageView(item)"
          >
            <v-icon medium class="mr-2"> mdi-eye-outline </v-icon>
          </v-btn>
          <v-btn
            color="red"
            class="mr-2 btn white--text"
            @click="opendialogDelete(item)"
          >
            <v-icon medium class="mr-2"> mdi-delete </v-icon>
          </v-btn>
        </template>
        <template v-slot:no-data>
          <v-btn color="primary" @click="initialize()"> Reset </v-btn>
        </template>
      </v-data-table>
    </div>
  </div>
</template>
<script>
import { mapActions, mapGetters } from "vuex";

export default {
  data: () => ({
    dialog: false,
    dialogDelete: false,
    loading: false,
    dialogimage: false,
    dialogresolve: false,
    dialogreverted: false,
    dialogDelete: false,
    dialogimageShow: false,
    search: "",
    headers: [
      { text: "name", value: "damage_type.name", sortable: true },
      { text: "Status", value: "status", sortable: true },
      { text: "Created By", value: "declared_by.username", sortable: true },
      { text: "Closed By", value: "closed_by.name", sortable: true },
      { text: "Created At", value: "created_at", sortable: true },
      { text: "Actions", value: "actions", sortable: false },
    ],
    damageByEquipments: [],
    equipmentsFiltre: [],
    showdetails: false,
    PhotoShow: {
      id: null,
      description: null,
      filename: "",
      damage_id: null,
      created_at: "",
      updated_at: "",
    },
    photo: {
      id: "",
      description: "",
      foreman_id: "",
      photos: [],
    },
    ImagesPath: "http://localhost:8000/storage/cdn/damagePhotos/",
    damageSelect: {
      id: null,
      status: "",
      description: "",
      declaredBy_id: null,
      declaredAt: "",
      confirmedBy_id: null,
      confirmedAt: null,
      closedBy_id: null,
      closedAt: null,
      revertedBy_id: null,
      revertedAt: null,
      revertedTimes: null,
      equipment_id: null,
      damage_type_id: null,
      created_at: "",
      updated_at: "",
      declared_by: {
        id: null,
        username: "",
        lastName: "",
        firstName: "",
        email: "",
        phoneNumber: "",
        fonction_id: null,
        created_at: "",
        updated_at: "",
        fonction: {
          id: null,
          name: "",
          department_id: null,
          created_at: "",
          updated_at: "",
          department: {
            id: null,
            name: "",
            created_at: "",
            updated_at: "",
          },
        },
      },
      confirmed_by: null,
      closed_by: null,
      reverted_by: null,
      equipment: {
        id: null,
        name: "",
        profile_group_id: null,
        created_at: "",
        updated_at: "",
        profile_group: {
          id: null,
          name: "",
          department_id: null,
          created_at: "",
          updated_at: "",
          department: {
            id: null,
            name: "",
            created_at: "",
            updated_at: "",
          },
        },
      },
      damage_type: {
        id: null,
        name: "",
        profile_group_id: null,
        department_id: null,
        created_at: "",
        updated_at: "",
        profile_group: {
          id: null,
          name: "",
          department_id: null,
          created_at: "",
          updated_at: "",
          department: {
            id: null,
            name: "",
            created_at: "",
            updated_at: "",
          },
        },
        department: {
          id: null,
          name: "",
          created_at: "",
          updated_at: "",
        },
      },
      photos: [
        {
          id: null,
          description: null,
          filename: "",
          damage_id: null,
          created_at: "",
          updated_at: "",
        },
      ],
    },
    equipment: null,
    idEquipment: null,
    EquipmentsByCounter: {
      id: null,
      nameEquipment: "",
      damagedCount: null,
      confirmedCount: null,
      closedCount: null,
    },
    confirmDamage: {
      id: null,
      confirmedBy_id: null,
      resolveDescription: "",
    },
    closeDamage: {
      id: null,
      closedBy_id: null,
    },
    revertDamage: {
      id: null,
      revertedBy_id: null,
      revertedDescription: "",
    },
    Damagedelete: {
      id: null,
    },
    editedIndex: -1,
    editedItem: {
      id: null,
      name: "",
      description: "",
    },
    defaultItem: {
      id: null,
      name: "",
      description: "",
    },
  }),
  mounted() {
    document.title = "user";
    this.loading = true;
    setTimeout(() => {
      this.initialize();
      this.loading = false;
    }, 2500);
  },
  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    },
    ...mapGetters([
      "getDamageTypeByEquipmentID",
      "getEquipmentsByCounter",
      "sendDamagePhotosStoragePath",
      "getUserActive",
    ]),
  },
  watch: {
    dialog(val) {},
  },
  created() {
    // this.initialize();
  },
  methods: {
    initialize() {
      this.idEquipment = localStorage.getItem("idEquipment");
      this.FindDamageTypeByEquipmentIDAction(this.idEquipment).then(() => {
        this.damageByEquipments = [...this.getDamageTypeByEquipmentID];
        this.damageByEquipments.map((item) => {
          if (item.equipment_id == this.idEquipment) {
            this.equipmentsFiltre.push(item);
          }
        });
      });

      this.getEquipmentsByCounterAction(this.idEquipment).then(() => {
        this.EquipmentsByCounter.id = this.getEquipmentsByCounter.id;
        this.EquipmentsByCounter.nameEquipment =
          this.getEquipmentsByCounter.nameEquipment;

        this.EquipmentsByCounter.damagedCount =
          this.getEquipmentsByCounter.damagedCount;
        this.EquipmentsByCounter.confirmedCount =
          this.getEquipmentsByCounter.confirmedCount;
        this.EquipmentsByCounter.closedCount =
          this.getEquipmentsByCounter.closedCount;
      });

      console.log("EquipmentsByCounter", this.EquipmentsByCounter);
    },
    ...mapActions([
      "FindDamageTypeByEquipmentIDAction",
      "getEquipmentsByCounterAction",
      "confirmDamageAction",
      "closeDamageAction",
      "revertDamageAction",
      "sendDamagePhotosStoragePathAction",
      "deleteDAMAGEAction",
    ]),
    pageView(item) {
      this.damageSelect = item;
      this.photo.id = item.id;

      console.log("this.damageSelect", this.damageSelect);
      this.dialog = true;
      this.showdetails = true;
    },
    closedtailedialoge() {
      this.showdetails = false;
      this.dialog = false;
    },
    opendialogresolve() {
      this.dialogresolve = true;
    },
    opendialogreverted() {
      this.dialogreverted = true;
    },
    opendialogDelete(item) {
      this.dialogDelete = true;
      this.Damagedelete.id = item.id;
    },
    confirmed() {
      this.confirmDamage.id = this.damageSelect.id;
      this.confirmDamage.confirmedBy_id = this.getUserActive.user.id;

      this.confirmDamageAction(this.confirmDamage).then(() => {});
      this.showdetails = false;
      this.dialogresolve = false;

      this.dialog = false;
    },
    closed() {
      this.closeDamage.id = this.damageSelect.id;
      this.closeDamage.closedBy_id = this.getUserActive.user.id;

      this.closeDamageAction(this.closeDamage).then(() => {});
      this.showdetails = false;
      this.dialog = false;
    },
    revert() {
      this.revertDamage.id = this.damageSelect.id;
      this.revertDamage.revertedBy_id = this.getUserActive.user.id;

      this.revertDamageAction(this.revertDamage).then(() => {});
      this.showdetails = false;
      this.dialogreverted = false;
      this.dialog = false;
    },
    deleteDamage() {
      this.deleteDAMAGEAction(this.Damagedelete).then(() => {
        this.damageByEquipments = this.damageByEquipments.filter((e) => {
          return e.id != this.Damagedelete.id;
        });
      });
      this.dialogDelete = false;
    },
    showImage(item) {
      console.log("image click", item);
      this.PhotoShow = item;
      this.dialogimageShow = true;
    },
    sendImage() {
      this.photo.foreman_id = this.getUserActive.user.id;

      var formData = new FormData();
      formData.append("id", parseFloat(this.photo.id));
      formData.append("description", this.photo.description);

      this.photo.photos.map((item) => {
        formData.append("photos[]", item);
      });
      formData.append(
        "foreman_id",
        parseFloat(parseFloat(this.photo.foreman_id))
      );

      console.log("this.photo", this.photo);
      this.sendDamagePhotosStoragePathAction(formData).then(() => {
        // this.foremanIntervention = [...this.sendDamagePhotosStoragePath];
        console.log("done");
      });
      this.dialogimage = false;
    },
  },
};
</script>
