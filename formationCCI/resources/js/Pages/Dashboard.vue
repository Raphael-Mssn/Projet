<template>
    <app-layout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Dashboard
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <!-- Affichage d'un petit message si la création de la formation est un succès -->
                <div class="bg-green-200 text-green-500 p-3" v-if="$page.props.flash.success">
                    {{ $page.props.flash.success }}
                </div>
                <!-- Fin affichage -->
                <div class="w-full">
                    <form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4" @submit.prevent="submit">
                        <div class="mb-4">
                            <label class="block text-gray-700 text-sm font-bold mb-2" for="title">
                                Titre de la formation
                            </label>
                            <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700
                            leading-tight focus:outline-none focus:shadow-outline" id="title" type="text" 
                            v-model="form.title">
                            <!-- Affichage message d'erreur si l'information n'est pas renseignée -->
                            <div class="bg-red-200 text-red-800 p-2 my-2" v-if="$page.props.errors.title">
                                {{ $page.props.errors.title }}
                            </div>
                            <!-- Fin affichage message d'erreur -->
                        </div>
                        <div class="mb-4">
                            <label class="block text-gray-700 text-sm font-bold mb-2" for="description">
                                Description de la formation
                            </label>
                            <textarea class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700
                            leading-tight focus:outline-none focus:shadow-outline" id="description" type="text" 
                            v-model="form.description">
                            </textarea>
                            <!-- Affichage message d'erreur si l'information n'est pas renseignée -->
                            <div class="bg-red-200 text-red-800 p-2 my-2" v-if="$page.props.errors.description">
                                {{ $page.props.errors.description }}
                            </div>
                            <!-- Fin affichage message d'erreur -->
                        </div>
                        <div class="mb-4">
                            <h2 class="text-2xl">Episodes de la formation</h2>
                            <div v-for="(episode, index) in form.episodes" v-bind:key="index">
                                
                                <!-- Titre de l'épisode -->
                                <label class="block text-gray-700 text-sm font-bold mb-2" :for="'title-'+index">
                                    Titre de l'épisode n° {{ index + 1 }}
                                </label>
                                <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700
                                leading-tight focus:outline-none focus:shadow-outline" :id="'title-'+index" 
                                type="text" v-model="form.episodes[index].title">
                                <!-- Affichage message d'erreur si l'information n'est pas renseignée -->
                                <div class="bg-red-200 text-red-800 p-2 my-2" 
                                v-if="$page.props.errors['episodes.' + index + '.title']">
                                    {{ $page.props.errors['episodes.' + index + '.title'] }}
                                </div>
                                <!-- Fin affichage message d'erreur -->

                                <!-- Description de l'épisode -->
                                <label class="block text-gray-700 text-sm font-bold mb-2" :for="'description-'
                                +index">
                                    Description de l'épisode n° {{ index + 1 }}
                                </label>
                                <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700
                                leading-tight focus:outline-none focus:shadow-outline" :id="'description-'+index" 
                                type="text" v-model="form.episodes[index].description">
                                <!-- Affichage message d'erreur si l'information n'est pas renseignée -->
                                <div class="bg-red-200 text-red-800 p-2 my-2" 
                                v-if="$page.props.errors['episodes.' + index + '.description']">
                                    {{ $page.props.errors['episodes.' + index + '.description'] }}
                                </div>
                                <!-- Fin affichage message d'erreur -->

                                <!-- URL de l'épisode -->
                                <label class="block text-gray-700 text-sm font-bold mb-2" :for="'video_url-'
                                +index">
                                    URL de la vidéo de l'épisode n° {{ index + 1 }}
                                </label>
                                <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700
                                leading-tight focus:outline-none focus:shadow-outline mb-5" :id="'video_url-'
                                +index" type="text" v-model="form.episodes[index].video_url">
                                <!-- Affichage message d'erreur si l'information n'est pas renseignée -->
                                <div class="bg-red-200 text-red-800 p-2 my-2" 
                                v-if="$page.props.errors['episodes.' + index + '.video_url']">
                                    {{ $page.props.errors['episodes.' + index + '.video_url'] }}
                                </div>
                                <!-- Fin affichage message d'erreur -->

                            </div>
                        </div>
                        <!-- Bouton d'ajout d'un épisode avec une limite d'épisodes maximal -->
                        <button class="bg-green-600 rounded py-2 px-4 my-2 text-white" 
                        v-if="form.episodes.length < 15" @click.prevent="add">
                            +
                        </button>
                        <!-- Séparation entre les deux boutons -->
                        <span v-if="form.episodes.length > 1"> | </span>
                        <!-- Bouton de suppression d'un épisode lors de la création de la formation -->
                        <button class="bg-red-600 rounded py-2 px-4 my-2 text-white" 
                        v-if="form.episodes.length > 1" @click.prevent="remove">
                            🗑️
                        </button> <br>
                        <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded
                        focus:outline-none focus:shadow-outline" type="submit">
                            Créer ma formation
                        </button>
                    </form>
                </div>
            </div>
        </div>
    </app-layout>
</template>

<script>
    import AppLayout from '@/Layouts/AppLayout';

    export default {
        components: {
            AppLayout
        },

        data() {
            return {
                form: {
                    title: null,
                    description: null,
                    episodes: [
                        {title: null, description: null, video_url: null}
                    ]
                }
            }
        },

        methods: {
            //Méthode pour submit le formulaire
            submit() {
                this.$inertia.post('/courses', this.form)
            },

            //Méthode pour ajouter un épisode à une formation
            add() {
                this.form.episodes.push({title: null, description: null, video_url: null});
            },

            //Méthode pour retirer un épisode à une formation en cours de création
            remove() {
                this.form.episodes.pop();
            }
        }
    }
</script>
