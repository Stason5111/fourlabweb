<template>
    <div class="leaderboard">
        <div class="header">Таблица лидеров</div>
        <div v-if="error" class="error">{{ error }}</div>
        <div v-if="isLoading" class="loading-message">Загрузка...</div>
        <ul v-else>
            <li v-for="entry in limitedSortedLeaderboard" :key="entry.username">
                {{ entry.username }} - {{ entry.trys }} попыток
            </li>
        </ul>
        <div v-if="limitedSortedLeaderboard.length === 0 && !isLoading" class="no-data-message">
            Нет данных для отображения лидеров.
        </div>
    </div>
    <div>
        <router-link to="/" class="no-underline">
            <PixelButton class="enter">
                НАЗАД
            </PixelButton>
        </router-link>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed, onMounted } from 'vue';
import PixelButton from "../components/FancyButton.vue";

interface LeaderboardEntry {
    username: string;
    trys: number;
}

export default defineComponent({
    name: 'Leaderboard',
    components: { PixelButton },
    setup() {
        const error = ref<string | null>(null);
        const isLoading = ref(true);
        const leaderboard = ref<LeaderboardEntry[]>([]);

        onMounted(() => {
            const storedLeaderboard = localStorage.getItem('leaderboard');

            if (storedLeaderboard) {
                const allEntries: LeaderboardEntry[] = JSON.parse(storedLeaderboard);

                // Создаем Map для удаления дубликатов
                const uniqueEntriesMap: Map<string, LeaderboardEntry> = new Map();

                allEntries.forEach(entry => {
                    // Если username уже существует, проверяем, чтобы сохранить запись с меньшим trys
                    if (!uniqueEntriesMap.has(entry.username) || uniqueEntriesMap.get(entry.username)!.trys > entry.trys) {
                        uniqueEntriesMap.set(entry.username, entry);
                    }
                });

                // Преобразуем Map обратно в массив
                leaderboard.value = Array.from(uniqueEntriesMap.values());
            } else {
                error.value = 'Нет данных для загрузки';
            }

            isLoading.value = false;
        });

        // Сортируем и ограничиваем до 20 записей
        const sortedLeaderboard = computed(() => {
            return leaderboard.value.sort((a, b) => a.trys - b.trys);
        });

        const limitedSortedLeaderboard = computed(() => {
            return sortedLeaderboard.value.slice(0, 20); // Ограничиваем до 20 записей
        });

        return {
            error,
            isLoading,
            limitedSortedLeaderboard,
        };
    },
});
</script>

<style scoped>
.header {
    font-size: 24px;
    font-weight: bold;
}

.error {
    color: red;
}

.loading-message {
    color: blue;
}

.no-data-message {
    color: gray;
    font-style: italic;
}
</style>