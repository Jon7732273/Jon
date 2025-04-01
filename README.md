#
import matplotlib.pyplot as plt
import geopandas as gpd

# Загрузка данных о странах мира
world = gpd.read_file(gpd.datasets.get_path('naturalearth_lowres'))

# Фильтрация данных только для Азии
asia_countries = world[world['continent'] == 'Asia']

# Определение регионов Зарубежной Азии
regions = {
    "Юго-Западная Азия": [
        "Иран", "Ирак", "Сирия", "Ливан", "Иордания", "Палестина",
        "Израиль", "Кувейт", "Бахрейн", "Оман", "Йемен", "Афганистан"
    ],
    "Восточная Азия": ["Монголия", "КНДР", "Юж. Корея", "Япония"],
    "Средняя Азия": ["Туркменистан", "Узбекистан", "Таджикистан", "Киргизия", "Азербайджан", "Грузия"],
    "Южная Азия": ["Индия", "Пакистан", "Бангладеш", "Шри-Ланка", "Непал", "Бутан", "Мальдивы"],
    "Юго-Восточная Азия": ["Китай", "Мьянма", "Таиланд", "Лaos", "Камбоджа", "Вьетнам", "Малайзия", "Бруней", "Сингапур", "Филиппины", "Индонезия", "Восточный Тимор"]
}

# Добавление столбца с регионами в геодатафрейм
def get_region(country_name):
    for region, countries in regions.items():
        if country_name in countries:
            return region
    return None

asia_countries['region'] = asia_countries['name'].apply(get_region)

# Цветовая палитра для регионов
region_colors = {
    "Юго-Западная Азия": "red",
    "Восточная Азия": "yellow",
    "Средняя Азия": "orange",
    "Южная Азия": "green",
    "Юго-Восточная Азия": "blue"
}

# Отображение карты
fig, ax = plt.subplots(figsize=(15, 10))

# Рисуем карту Азии с цветами по регионам
asia_countries.plot(ax=ax, column='region', cmap=region_colors, edgecolor='black')

# Добавляем названия стран
for idx, row in asia_countries.iterrows():
    if row['name'] != 'Russia':  # Исключаем Россию из центрирования
        plt.annotate(text=row['name'], xy=row['geometry'].centroid.coords[0], horizontalalignment='center')

# Настройки графика
ax.set_title("Картосхема Зарубежной Азии", fontsize=16)
ax.set_axis_off()  # Отключаем оси

# Отображаем карту
plt.show()
