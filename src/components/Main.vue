<!--
    Awalnya mau saya pisah-pisah seperti Proyek, Anggota, Modal, dll
    Tapi karena error terus jadinya satu components saja :)
-->
<template>
    <div class="container mt-5">
        <div v-if="projects.length === 0" class="alert alert-info mt-3">Tidak ada project yang sedang berlangsung</div>

        <!-- Modal Tambah Project-->
        <div class="modal fade" id="tambahProyek" tabindex="-1" aria-labelledby="tambahProyekLabel" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content">
                <div class="modal-header">
                    <h1 class="modal-title fs-5" id="tambahProyekLabel">Tambah Proyek</h1>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                
                <div class="modal-body">
                    <form @submit.prevent="addProject" class="form-floating">
                    <div class="form-floating mb-3">
                        <input v-model="newProject.name" id="nameInput" type="text" class="form-control" placeholder="Nama Proyek" required>
                        <label for="nameInput">Nama Proyek</label>
                    </div>
                    <div class="form-floating mb-3">
                        <input v-model="newProject.leader" id="leaderInput" type="text" class="form-control" placeholder="Ketua Proyek" required>
                        <label for="leaderInput">Ketua Proyek</label>
                    </div>
                    <div class="form-floating mb-3">
                        <input v-model="newProject.startDate" id="startDateInput" type="date" class="form-control" placeholder="Tanggal Mulai" required>
                        <label for="startDateInput">Tanggal Mulai</label>
                    </div>
                    <div class="form-floating mb-3">
                        <input v-model="newProject.endDate" id="endDateInput" type="date" class="form-control" placeholder="Tanggal Selesai" required>
                        <label for="endDateInput">Tanggal Selesai</label>
                    </div>
                    <div class="form-floating mb-3">
                        <textarea v-model="newProject.description" id="descriptionInput" class="form-control" placeholder="Deskripsi Proyek" required></textarea>
                        <label for="descriptionInput">Deskripsi Proyek</label>
                    </div>

                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Tutup</button>
                        <button type="submit" class="btn btn-primary">Tambah</button>
                    </div>
                    </form>
                </div>
                </div>
            </div>
        </div>

        <!-- Modal Edit Project -->
        <div class="modal fade" id="editProyek" tabindex="-1" aria-labelledby="editProyekLabel" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content">
                    <div class="modal-header">
                        <h1 class="modal-title fs-5" id="editProyekLabel">Edit Proyek</h1>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <form @submit.prevent="updateProject(editIndex)" class="form-floating">
                            <div class="form-floating mb-3">
                                <input v-model="editProject.name" id="editNameInput" type="text" class="form-control" placeholder="Nama Proyek" required>
                                <label for="editNameInput">Nama Proyek</label>
                            </div>
                            <div class="form-floating mb-3">
                                <input v-model="editProject.leader" id="editLeaderInput" type="text" class="form-control" placeholder="Ketua Proyek" required>
                                <label for="editLeaderInput">Ketua Proyek</label>
                            </div>
                            <div class="form-floating mb-3">
                                <input v-model="editProject.startDate" id="editStartDateInput" type="date" class="form-control" placeholder="Tanggal Mulai" required>
                                <label for="editStartDateInput">Tanggal Mulai</label>
                            </div>
                            <div class="form-floating mb-3">
                                <input v-model="editProject.endDate" id="editEndDateInput" type="date" class="form-control" placeholder="Tanggal Selesai" required>
                                <label for="editEndDateInput">Tanggal Selesai</label>
                            </div>
                            <div class="form-floating mb-3">
                                <textarea v-model="editProject.description" id="editDescriptionInput" class="form-control" placeholder="Deskripsi Proyek" required></textarea>
                                <label for="editDescriptionInput">Deskripsi Proyek</label>
                            </div>
                            <div class="modal-footer">
                                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Tutup</button>
                                <button type="submit" class="btn btn-primary">Simpan</button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>

        <!-- Modal Hapus Project -->
        <div class="modal fade" id="deleteProyek" tabindex="-1" aria-labelledby="deleteProyekLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="deleteProyekLabel">Hapus Project</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>

                    <div class="modal-body">
                        Yakin mau menghapus project?
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Keluar</button>
                        <button type="button" class="btn btn-danger" @click="deleteP" data-bs-dismiss="modal">Hapus</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Modal Tambah Anggota -->
        <div class="modal fade" id="tambahMember" tabindex="-1" aria-labelledby="tambahMemberLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="tambahMemberLabel">Tambah Anggota</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>

                    <div class="modal-body">
                        <form @submit.prevent="addMember(addMemberProjectIndex)">
                            <div class="mb-3">
                            <label for="memberName" class="form-label">Nama Anggota</label>
                            <input v-model="newMember.name" type="text" class="form-control" id="memberName" required>
                            </div>

                            <div class="modal-footer">
                                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Keluar</button>
                                <button type="submit" class="btn btn-primary">Tambah</button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>

        <!-- Modal Edit Anggota -->
        <div class="modal fade" id="editMember" tabindex="-1" aria-labelledby="editMemberLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="editMemberLabel">Edit Anggota</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <form @submit.prevent="updateMember">
                            <div class="mb-3">
                                <label for="editMemberName" class="form-label">Nama Anggota</label>
                                <input type="text" class="form-control" id="editMemberName" v-model="editMember.name">
                            </div>
                            <div class="modal-footer">
                                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Tutup</button>
                                <button type="submit" class="btn btn-primary">Simpan</button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>

        <!-- Modal Hapus Anggota -->
        <div class="modal fade" id="deleteMember" tabindex="-1" aria-labelledby="deleteMemberLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="deleteMemberLabel">Hapus Anggota?</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>

                    <div class="modal-body">
                        Yakin mau menghapus anggota?
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Keluar</button>
                        <button type="button" class="btn btn-danger" @click="deleteConfirmed()" data-bs-dismiss="modal">Hapus</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Project -->
        <div class="row row-cols-1 row-cols-md-3 g-4">
            <transition-group name="project" tag="div" v-for="(project, index) in projects" :key="project.id" class="col-md-6">
                <div class="card" style="height: 380px; width: 630px;">
                    <h2 class="card-header bg-primary">{{ project.name }}</h2>
                    <div class="card-body">
                        <div class="row">
                            <div class="col-6">
                                <p class="card-title">{{ project.description }}</p>
                                <p class="card-text">{{ project.startDate }} - {{ project.endDate }}</p>
                                <hr>

                                <div class="aksi-proyek">
                                    <!-- Edit -->
                                    <button class="btn btn-sm text-primary" data-bs-toggle="modal" data-bs-target="#editProyek" @click="startEditProject(index)">
                                        Edit <i class="fas fa-edit"></i>
                                    </button>
                                    <!-- Hapus -->
                                    <button class="btn btn-sm text-danger" data-bs-toggle="modal" data-bs-target="#deleteProyek" @click="setDeleteProjectIndex(index)">
                                        Hapus <i class="fas fa-trash"></i>
                                    </button>
                                </div>

                                <div class="col position-absolute bottom-0 start-1">
                                        <h4 class="text-muted mb-0">Leader</h4>
                                        <h5>{{ project.leader }}</h5>
                                </div>
                            </div>

                            <div class="col" style="max-height: 250px; overflow-y: auto;">
                                <h4>
                                    Anggota
                                    <button class="btn text-success" @click="showAddMemberForm(index)" data-bs-toggle="modal" data-bs-target="#tambahMember"><i class="fa-solid fa-plus"></i></button>
                                </h4>

                                <!-- Anggota -->
                                <transition-group name="list" tag="div">
                                    <div v-for="(member, memberIndex) in project.members" :key="member.id" class="member-item">
                                        <div class="d-flex justify-content-between">
                                            <h5>{{ member.name }}</h5>
                                                <div class="aksi">
                                                <!-- Edit -->
                                                <button class="btn btn-sm text-primary" data-bs-toggle="modal" data-bs-target="#editMember" @click="startEditMember(memberIndex, index)">
                                                <i class="fas fa-edit"></i>
                                                </button>

                                                <!-- Hapus -->
                                                <button class="btn btn-sm text-danger" data-bs-toggle="modal" data-bs-target="#deleteMember" @click="deleteMember(index, memberIndex)">
                                                    <i class="fas fa-trash"></i>
                                                </button>
                                            </div>
                                        </div>
                                    </div>
                                </transition-group>
                            </div>
                        </div>
                    </div>
                </div>
            </transition-group>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Modal',
    components: {
        
    },
    data() {
        return {
        newProject: {
            name: '',
            leader: '',
            startDate: '',
            endDate: '',
            description: '',
            members: []
        },
        newMember: {
            name: '',
        },
            projects: [],
            editIndex: null,
            editType: null,
            editProject: {},
            deleteProjectIndex: null,
        
            editMemberIndex: null,
            deleteMemberIndex: null,
            editProjectIndex: null,
            editMember: {},
            deleteMemberProjectIndex: null,

            addMemberFormVisible: true,
            addMemberProjectIndex: null,
        }
    },
    methods: {
        // Project
        addProject() {
        this.projects.push({
            id: Date.now(),
            name: this.newProject.name,
            leader: this.newProject.leader,
            startDate: this.newProject.startDate,
            endDate: this.newProject.endDate,
            description: this.newProject.description,
            members: []
        });
            this.newProject.name = '';
            this.newProject.leader = '';
            this.newProject.startDate = '';
            this.newProject.endDate = '';
            this.newProject.description = '';

            this.hideModal('tambahProyek');
        },

        startEditProject(index) {
            this.editIndex = index;
            this.editType = 'project';
            this.editProject = Object.assign({}, this.projects[index]);
        },

        updateProject(index) {
            this.projects.splice(index, 1, this.editProject);
            this.editIndex = null;
            this.editType = null;
            this.editProject = {};
            this.hideModal('editProyek');
        },

        setDeleteProjectIndex(index) {
            this.deleteProjectIndex = index;
        },

        deleteP(index) {
            if (this.deleteProjectIndex !== null) {
                this.projects.splice(this.deleteProjectIndex, 1);
                this.deleteProjectIndex = null;
            }
        },

        // Anggota
        addMember(projectIndex) {
            if (projectIndex >= 0 && projectIndex < this.projects.length) {
                const newMemberData = {
                id: Date.now(),
                name: this.newMember.name,
                tasks: []
                };

                if (!this.projects[projectIndex].hasOwnProperty('members')) {
                    this.$set(this.projects[projectIndex], 'members', []);
                }
                this.projects[projectIndex].members.push(newMemberData);
                this.newMember.name = '';
                this.addMemberFormVisible = false;
                this.addMemberProjectIndex = null;
            }
            this.hideModal('tambahMember');
        },

        showAddMemberForm(projectIndex) {
            this.addMemberFormVisible = true;
            this.addMemberProjectIndex = projectIndex;
        },

        startEditMember(memberIndex, projectIndex) {
            this.editMemberIndex = memberIndex;
            this.editMemberProjectIndex = projectIndex;
            this.editMember = { ...this.projects[projectIndex].members[memberIndex] };
        },

        updateMember() {
            if (this.editMemberIndex !== null && this.editMemberProjectIndex !== null) {
                this.projects[this.editMemberProjectIndex].members.splice(this.editMemberIndex, 1, this.editMember);
                this.editMemberIndex = null;
                this.editMemberProjectIndex = null;
                this.editMember = {};
                this.hideModal('editMember');
            }
        },

        deleteMember(projectIndex, memberIndex) {

            this.deleteMemberIndex = memberIndex;
            this.deleteMemberProjectIndex = projectIndex;
        },

        deleteConfirmed() {
            if (this.deleteMemberIndex !== null && this.deleteMemberProjectIndex !== null) {
                this.projects[this.deleteMemberProjectIndex].members.splice(this.deleteMemberIndex, 1);

                this.deleteMemberIndex = null;
                this.deleteMemberProjectIndex = null;

                this.hideModal('deleteMember');
            }
        },

        // Modal
        hideModal(modalId) {
            const modal = document.getElementById(modalId);
            const modalInstance = bootstrap.Modal.getInstance(modal);
            modalInstance.hide();
        },
    }
};
</script>

<style>
    .project-enter-active, .project-leave-active {
        transition: all 1s;
    }
    .project-enter-from, .project-leave-to {
        transform: scale(0);
    }

    .list-enter-active, .list-leave-active {
        transition: all 1s !important;
    }
    .list-enter-from, .list-leave-to {
        opacity: 0 !important;
        transform: translateY(-30px) !important;
    }

    .modal{
        background-color: rgba(0, 0, 0, 0.5);
    }

    .card-header{
        color: white;
    }
</style>
