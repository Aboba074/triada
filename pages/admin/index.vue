<template>
    <!-- Добавление новой услуги -->
    <FormKit @submit="addService()" type="form" :actions="false" messages-class="hidden" form-class="flex flex-col gap-6 items-center justify-center">
        <p class="mainHeading w-full">Добавление новой услуги</p>
        <FormKit v-model="servicesForm.title" validation="required" messages-class="text-[#E9556D] font-Cormorant" type="text" placeholder="Наименование услуги" name="Наименование услуги" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-transparent rounded-xl border border-white/15 w-full transition-all duration-500 focus:border-white focus:bg-[#191919]"/>
        <div class="flex flex-col gap-6 w-full md:w-2/3 lg:w-1/2 rounded-xl border border-white/10 p-4">
            <div class="flex flex-col gap-2">
                <p>Наименование иконки</p>
                <FormKit v-model="servicesForm.icon" validation="required" messages-class="text-[#E9556D] font-Cormorant" type="text" placeholder="Иконка" name="Иконка" outer-class="w-full" input-class="focus:outline-none px-4 py-2 bg-transparent rounded-xl border border-white/15 w-full transition-all duration-500 focus:border-white focus:bg-[#191919]"/>
            </div>
            <div class="flex items-center gap-2">
                <p>Отображение иконки</p>
                <p>-</p>
                <Icon class="text-3xl text-[#673ab7]" :name="servicesForm.icon"/>
            </div>
            <NuxtLink target="_blank" class="w-fit flex flex-col after:w-full after:h-px after:bg-white after:transition-all after:duration-500 hover:after:w-0" to="https://icon-sets.iconify.design/">Список доступных иконок</NuxtLink>
        </div>
        <FormKit validation="required" accept="image/*" @change="(e) => {files.servicesMain = e.target.files[0]; console.log('Выбрано main:', files.servicesMain);}" messages-class="text-[#E9556D] font-Cormorant" type="file" :validation-messages="{required: 'Основное изображение обязательно'}" label="Основное изображение" name="servicesMain" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-transparent rounded-xl border border-white/15 w-full transition-all duration-500 focus:border-white focus:bg-[#191919]"/>
        <FormKit validation="required" accept="image/*" @change="(e) => {files.servicesAction = e.target.files[0];  console.log('Выбрано action:', files.servicesAction);}" messages-class="text-[#E9556D] font-Cormorant" type="file" :validation-messages="{required: 'Дополнительное изображение обязательно'}" label="Дополнительное изображение" name="servicesAction" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-transparent rounded-xl border border-white/15 w-full transition-all duration-500 focus:border-white focus:bg-[#191919]"/>
        <FormKit v-model="servicesForm.shortDesc" validation="required" messages-class="text-[#E9556D] font-Cormorant" type="textarea" placeholder="Краткое описание" name="Краткое описание" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-transparent rounded-xl border border-white/15 w-full transition-all duration-500 focus:border-white focus:bg-[#191919]"/>
        <FormKit v-model="servicesForm.fullDesc" validation="required" messages-class="text-[#E9556D] font-Cormorant" type="textarea" placeholder="Полное описание" name="Полное описание" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-transparent rounded-xl border border-white/15 w-full transition-all duration-500 focus:border-white focus:bg-[#191919]"/>
        <div class="flex gap-4 flex-col w-full md:w-2/3 lg:w-1/2 rounded-xl border border-white/10 p-4" v-for="(advantage, index) in servicesForm.advantages" :key="index">
            <div class="flex items-center justify-between gap-4">
                <p>Преимущество № {{ index+1 }}</p>
                <button @click="removeAdvantage(index)" type="button">
                    <Icon class="text-3xl" name="material-symbols:delete-forever-rounded"/>
                </button>
            </div>
            <FormKit :name="`Наименование преимущества ${index+1}`" v-model="servicesForm.advantages[index].title" validation="required" messages-class="text-[#E9556D] font-Cormorant" type="text" placeholder="Наименование преимущества" outer-class="w-full" input-class="focus:outline-none px-4 py-2 bg-transparent rounded-xl border border-white/15 w-full transition-all duration-500 focus:border-white focus:bg-[#191919]"/>
            <FormKit :name="`Описание преимущества  ${index+1}`" v-model="servicesForm.advantages[index].description" validation="required" messages-class="text-[#E9556D] font-Cormorant" type="textarea" placeholder="Описание преимущества" outer-class="w-full" input-class="focus:outline-none px-4 py-2 bg-transparent rounded-xl border border-white/15 w-full transition-all duration-500 focus:border-white focus:bg-[#191919]"/>
        </div>
        <button @click="addAdvantage()" type="button" class="px-4 py-2 border border-[#673ab7] hover:bg-[#673ab7] hover:text-white rounded-full w-fit text-center transition-all duration-500 text-[#673ab7] bg-transparent">Добавить преимущество</button>
        <div class="flex gap-4 flex-col w-full md:w-2/3 lg:w-1/2 rounded-xl border border-white/10 p-4" v-for="(stage, index) in servicesForm.stages" :key="index">
            <div class="flex items-center justify-between gap-4">
                <p>Стадия № {{ index+1 }}</p>
                <button @click="removeStage(index)" type="button">
                    <Icon class="text-3xl" name="material-symbols:delete-forever-rounded"/>
                </button>
            </div>
            <FormKit :name="`Наименование стадии ${index+1}`" v-model="servicesForm.stages[index].title" validation="required" messages-class="text-[#E9556D] font-Cormorant" type="text" placeholder="Наименование стадии" outer-class="w-full" input-class="focus:outline-none px-4 py-2 bg-transparent rounded-xl border border-white/15 w-full transition-all duration-500 focus:border-white focus:bg-[#191919]"/>
            <FormKit :name="`Описание стадии ${index+1}`" v-model="servicesForm.stages[index].description" validation="required" messages-class="text-[#E9556D] font-Cormorant" type="textarea" placeholder="Описание стадии" outer-class="w-full" input-class="focus:outline-none px-4 py-2 bg-transparent rounded-xl border border-white/15 w-full transition-all duration-500 focus:border-white focus:bg-[#191919]"/>
        </div>
        <button @click="addStage()" type="button" class="px-4 py-2 border border-[#673ab7] hover:bg-[#673ab7] hover:text-white rounded-full w-fit text-center transition-all duration-500 text-[#673ab7] bg-transparent">Добавить стадию</button>
        <button :disabled="isSubmitting" :class="isSubmitting ? 'opacity-50' : 'hover:text-[#673ab7] hover:bg-transparent'" type="submit" class="px-4 py-2 mt-10 border border-[#673ab7] bg-[#673ab7] text-white rounded-full w-fit text-center transition-all duration-500">
            <span v-if="!isSubmitting">Добавить</span>
            <span v-else>Добавление...</span>
        </button>
    </FormKit>

    <!-- Редактирование услуг -->
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Редактирование услуг</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            <div v-for="service in services"
                class="flex flex-col gap-4 bg-[#252525] rounded-xl p-6 border border-white/10 shadow-[0px_0px_13px_-7px_white] transition-all duration-500 hover:shadow-none">
                <div class="flex items-center gap-4 self-end">
                    <NuxtLink :to="`/admin/edit-${service.id}`">
                        <Icon class="text-3xl text-amber-500" name="material-symbols:edit-outline" />
                    </NuxtLink>
                    <button :disables="isDeleting" :class="{isDeleting : 'opacity-50'}" @click="deleteService(service.id)" class="transition-all duration-500">
                        <Icon class="text-3xl text-red-500" name="material-symbols:delete-forever"/>
                    </button>
                </div>
                <div class="w-16 h-16 flex items-center justify-center p-2 bg-[#673ab7] rounded-full">
                    <Icon class="text-3xl" :name="service.icon" />
                </div>
                <span class="font-medium">{{ service.title }}</span>
                <span class="text-sm text-gray-400">{{ service.shortDesc }}</span>
            </div>
        </div>
    </div>

    <!-- Смена статуса для заявок -->
    <div class="flex flex-col gap-6">
        <div class="flex max-lg:flex-col lg:items-center justify-between gap-4">
            <p class="mainHeading">Заявки</p>
            <FormKit type="form" :actions="false" messages-class="hidden" form-class="flex max-md:flex-col gap-4 md:items-center">
                <FormKit v-model="selectedService" messages-class="text-[#E9556D] font-Cormorant" type="select" :options="servicesTitles" placeholder="Наименование услуги" name="Наименование услуги" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-transparent rounded-xl border border-white/15 w-full transition-all duration-500 focus:border-white focus:bg-[#191919]"/>
                <div class="flex items-center gap-2 md:gap-4 max-md:w-full">
                    <button :disabled="!selectedService" type="button" @click="filterBids()" class="px-4 py-2 border border-[#673ab7] bg-[#673ab7] text-white rounded-full w-1/2 md:w-fit text-center transition-all duration-500 " :class="selectedService ? 'hover:text-[#673ab7] hover:bg-transparent' : 'opacity-50'">Применить</button>
                    <button :disabled="!selectedService" type="button" @click="cancelFilterBids()" class="px-4 py-2 border border-[#673ab7] bg-transparent text-[#673ab7] rounded-full w-1/2 md:w-fit text-center transition-all duration-500" :class="selectedService ? 'hover:bg-[#673ab7] hover:text-white' : 'opacity-50'">Отменить</button>
                </div>
            </FormKit>
        </div>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
            <div class="flex flex-col gap-4 rounded-xl border border-white/10 bg-[#252525] shadow-[0px_0px_13px_-7px_white] p-4" v-for="bid in filteredBids" :key="bid.id">
                <div v-if="showConfirmButton === bid.id" class="flex flex-col gap-2 text-lg">
                    <button @click="updateBidStatus(bid.id, 'Отменена'); showConfirmButton = null"
                        class="px-4 py-1.5 border border-red-500 bg-red-500 text-white rounded-full w-fit text-center transition-all duration-500 hover:text-red-500 hover:bg-transparent">
                        Отменить
                    </button>
                    <button @click="updateBidStatus(bid.id, 'Подтверждена'); showConfirmButton = null"
                        class="px-4 py-1.5 border border-green-500 bg-green-500 text-white rounded-full w-fit text-center transition-all duration-500 hover:text-green-500 hover:bg-transparent">
                        Подтвердить
                    </button>
                    <button @click="showConfirmButton = null"
                        class="px-4 py-1.5 border border-white bg-white text-[#252525] rounded-full w-fit text-center transition-all duration-500 hover:text-white hover:bg-transparent">
                        Назад
                    </button>
                </div>
                <button v-else-if="bid.status == 'Новая'" @click="showConfirmButton = bid.id" class="self-end">
                    <Icon class="text-3xl text-amber-500" name="uil:exchange" />
                </button>
                <div class="flex flex-col gap-2 mt-auto">
                    <p><span class="text-[#673ab7] font-semibold">ID:</span> {{ bid.id }}</p>
                    <p><span class="text-[#673ab7] font-semibold">Пользователь:</span> {{ bid.users.surname }} {{ bid.users.name }} {{ bid.users.patronymic }}</p>
                    <p><span class="text-[#673ab7] font-semibold">Email:</span> {{ bid.users.email }}</p>
                    <p><span class="text-[#673ab7] font-semibold">Телефон:</span> {{ bid.users.phone }}</p>
                    <p><span class="text-[#673ab7] font-semibold">Услуга:</span> {{ bid.services.title }}</p>
                    <p><span class="text-[#673ab7] font-semibold">Статус:</span> {{ bid.status }}</p>
                </div>
            </div>
        </div>
    </div>

    <!-- Отзывы -->
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Отзывы</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
            <div class="flex flex-col gap-4 rounded-xl border border-white/10 bg-[#252525] shadow-[0px_0px_13px_-7px_white] p-4" v-for="review in refReviews" :key="review.id">
                <button class="self-end" @click="deleteReviews(review.id)">
                    <Icon class="text-3xl text-red-500" name="material-symbols:tab-close-inactive"/>
                </button>
                <p><span class="text-[#673ab7] font-semibold">Пользователь:</span> {{ review.users.surname }} {{ review.users.name }} {{ review.users.patronymic }}</p>
                <p><span class="text-[#673ab7] font-semibold">Услуга:</span> {{ review.services.title }}</p>
                <p><span class="text-[#673ab7] font-semibold">Текст:</span> {{ review.text }}</p>
            </div>
        </div>
    </div>
</template>

<script setup>
    /* название и язык страницы */
    useSeoMeta({
        title: 'Админ панель',
        lang: 'ru'
	})


    /* подключение БД и получение услуг */
    const supabase = useSupabaseClient()

    const services = ref([])
    const servicesTitles = ref([])
    const loadServices = async () => {
        try {
            const { data, error } = await supabase
            .from('services')
            .select('*')   
            .order('id', { ascending: true })

            if (error) throw error
            
            services.value = data // Загрузка данных
            servicesTitles.value = [...services.value.map(service => service.title)] // Наименования услуг

        } catch (error) {
            console.error('Ошибка загрузки:', error)
            showMessage('Не удалось загрузить данные', false)
        }
    }


    /* создание формы услуги */
    const servicesForm = ref({
        title: "",
        icon: "",
        shortDesc: "",
        fullDesc: "",
        servicesMain: "", 
        servicesAction: "",
        advantages: ref([{ title: '', description: '' }]),
        stages: ref([{ title: '', description: '' }])
    }) 

    //изображения
    const files = ref({
        servicesMain: null, 
        servicesAction: null
    })


    /* изменение преимуществ и стадий */
    const addItem = (list) => {
        list.value.push({ title: '', description: '' })
    }

    const removeItem = (list, index) => {
        if (list.value.length > 1) {
            list.value.splice(index, 1)
        }
    }

    // Используем универсальные функции для преимуществ и стадий
    const addAdvantage = () => addItem(computed(() => servicesForm.value.advantages))
    const removeAdvantage = (index) => removeItem(computed(() => servicesForm.value.advantages), index)

    const addStage = () => addItem(computed(() => servicesForm.value.stages))
    const removeStage = (index) => removeItem(computed(() => servicesForm.value.stages), index)


    /* добавление данных */
    // Функция очистки формы
    const resetForm = () => {
        servicesForm.value = {
            title: "",
            icon: "",
            shortDesc: "",
            fullDesc: "",
            servicesMain: "",
            servicesAction: "",
            advantages: [{ title: '', description: '' }],
            stages: [{ title: '', description: '' }]
        }
        files.value = {
            servicesMain: null,
            servicesAction: null
        }
    }

    // Реактивная переменная для отслеживания состояния отправки
    const isSubmitting = ref(false)

    // Функция отправки
    const addService = async () => {
        try {
            isSubmitting.value = true
            // Параллельная загрузка файлов
            const uploadResults = await Promise.all(
                Object.entries(files.value).map(async ([key, file]) => {
                    if (!file) {
                        console.error(`Файл для ${key} не выбран`)
                        return null
                    }

                    const extension = file.name.split('.').pop()
                    const uniqueName = `${Date.now()}_${key}_${Math.random()
                        .toString(36)
                        .substring(2, 7)}.${extension}`

                    const { data, error } = await supabase.storage
                        .from('images')
                        .upload(`${key}/${uniqueName}`, file)

                    if (error) throw error

                    return { key, fileName: uniqueName } // Возвращаем только имя файла
                })
            )

            // Обновляем форму с именами файлов
            uploadResults.forEach(result => {
                if (result) {
                    servicesForm.value[result.key] = result.fileName
                }
            })

            // Вставка данных в таблицу
            const { data, error } = await supabase
                .from('services')
                .insert([servicesForm.value])
                .select()

            if (error) throw error

            showMessage("Услуга добавлена!", true)
            await loadServices()
            resetForm()

        } catch (error) {
            console.error("Ошибка:", error)
            showMessage(error.message || "Ошибка при сохранении", false)
        } finally {
            isSubmitting.value = false
        }
    }


    /* удаление услуги */
    const isDeleting = ref(false)
    const deleteService = async (serviceId) => {
        try {
            isDeleting.value = true

            // Получаем данные услуги
            const { data: serviceData, error: fetchError } = await supabase
                .from('services')
                .select('servicesMain, servicesAction')
                .eq('id', serviceId)
                .single()

            if (fetchError) throw fetchError

            // Формируем список файлов с указанием папок
            const filesToDelete = []

            if (serviceData.servicesMain) {
                filesToDelete.push(`servicesMain/${serviceData.servicesMain}`)
                console.log('Добавлен файл для удаления:', `servicesMain/${serviceData.servicesMain}`)
            }

            if (serviceData.servicesAction) {
                filesToDelete.push(`servicesAction/${serviceData.servicesAction}`)
                console.log('Добавлен файл для удаления:', `servicesAction/${serviceData.servicesAction}`)
            }

            // Пакетное удаление файлов
            if (filesToDelete.length > 0) {
                const { error: storageError } = await supabase.storage
                    .from('images')
                    .remove(filesToDelete)

                if (storageError) throw storageError
            }

            // Удаляем запись из БД
            const { error: deleteError } = await supabase
                .from('services')
                .delete()
                .eq('id', serviceId)

            if (deleteError) throw deleteError

            // Обновляем список
            await loadServices()
            showMessage('Услуга удалена!', true)

        } catch (error) {
            console.error('Ошибка:', error)
            showMessage(`Ошибка удаления: ${error.message}`, false)
        } finally {
            isDeleting.value = false
        }
    }


    /* заявки */
    const { data:bids, error:bidsError } = await supabase
    .from('bids')
    .select('*, services(*), users(*)')   
    .order('id', { ascending: true })
    
    const refBids = ref(bids) // Исходный список заявок
    const filteredBids = ref([...refBids.value]) // Отфильтрованный список заявок

    // Состояние для выбранной услуги
    const selectedService = ref('')

    // Функция для фильтрации заявок
    const filterBids = () => {
        if (!selectedService.value) {
            // Если поле пустое, показываем все заявки
            filteredBids.value = [...refBids.value]
        } else {
            // Фильтруем заявки по выбранной услуге
            filteredBids.value = refBids.value.filter((bid) =>
                bid.services?.title === selectedService.value
            )
        }
    }

    // Функция для сброса фильтрации
    const cancelFilterBids = () => {
        selectedService.value = '' // Очищаем выбранное значение
        filteredBids.value = [...refBids.value] // Показываем все заявки
    }

    
    /* создание сообщений и роутера */
    const { showMessage } = useMessagesStore()


    /* изменение статуса */
    const showConfirmButton = ref(null); // Подтверждение

    // Универсальная функция для обновления статуса заявки
    const updateBidStatus = async (bidId, status) => {
        const { data, error } = await supabase
            .from('bids')
            .update({ status })
            .eq('id', bidId)

        if (error) {
            console.error(error)
            showMessage("Произошла ошибка!", false)
        } else {
            // Обновление статуса в интерфейсе
            const bidIndex = refBids.value.findIndex((bid) => bid.id === bidId)
            if (bidIndex !== -1) {
                refBids.value[bidIndex].status = status
            }
            showMessage(`Заявка ${status.toLowerCase()}!`, true)
        }
    }


    /* отзывы */
    const { data:reviews, error:reviewsError } = await supabase
    .from('reviews')
    .select('*, services(*), users(*)')   
    .order('id', { ascending: true })
    
    const refReviews = ref(reviews)

    const deleteReviews = async (reviewId) => {        
        const { error } = await supabase
        .from('reviews')
        .delete()
        .eq('id', reviewId)
          
        if (error) {
            console.error(error)
            showMessage("Произошла ошибка!", false)
        } else {
            // Удаляем запись из массива refReviews
            const reviewIndex = refReviews.value.findIndex((review) => review.id === reviewId);
            if (reviewIndex !== -1) {
                refReviews.value.splice(reviewIndex, 1);
            }
            showMessage("Отзыв удалён", true)
        }
    }


    /* инициализация */
    onMounted(() => {
        loadServices()
    })
</script>