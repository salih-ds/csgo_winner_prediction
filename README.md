# csgo_winner_prediction
Team 2 win probability prediction

Ответы с вероятностью победы команды 2 храняться в subs

## Данные
Было сгенерировано много признаков, полный список признаков:
- id - идентификатор игрока
- total_kills - общее количество убийств
- headshots - хедшоты %
- total_deaths - общее количество сметрей
- kd_ratio - соотношение убийств к сметрям
- damage_per_round - ср. урон за раунд
- grenade_damage_per_round - ср. урон гранатой за раунд
- maps_played - количество сыгранных карт
- rounds_played - количество сыгранных раундов
- kills_per_round - ср. убийств за раунд
- assists_per_round - ср. ассистов за раунд
- deaths_per_round - ср. смертей за раунд
- saved_by_teammate_per_round - ср. спасен тиммейтом за раунд
- saved_teammates_per_round - ср. спасение тиммейта за раунд
- rating - RATING 2.0 из выгрузки HLTV
- kill_death - убиств/смертей (почти полностью дублирует kd_ratio)
- kill_round - убийств/раунд (есть ошибки пересчитать по формуле)
- rounds_with_kills - всего раундов с убийством
- kill_death_difference - всего убийств - всего смертей
- total_opening_kills - количество 1-х убийств за раунд
- total_opening_deaths - количество 1-х смертей за раунд
- opening_kill_ratio - 1-х убийств / 1-х сметрей
- opening_kill_rating - метрика HLTV 
- team_win_percent_after_first_kill - доля выигранных раундов если игрок сделал первое убийство
- first_kill_in_won_rounds - в какой доле выигранных раундов игрок совершил первое убийство
- team_id - идентификатор команды (есть игроки, игравшие в нескольких командах)
- map_name - название карты
- map_id - идентификатор игры на карте (для всех игр по 2 идентификатора, тк в игре участвуют 2 команды) (не имеет привязки к дате, но в тестовых данных игры сыграны после тренировочных)
- shot_damage - урон от выстрелов = урона за раунд - урон гранатой за раунд
- shot_damage_percent - соотношение урона от выстрелов и урона гранатой от общего
- grenade_damage_percent - среднее количество сыгранных раундов в игру
- rounds_with_kill_per_all_rounds - соотношение раундов с убийством ко всем раундам
- kill_death_difference_per_all_plays - соотношение убийств-сметрей / всего игр
- saved_per_saved_by - спасение тиммейта / спасен тиммейтом
- opening_kills_per_death - первых убийств / первых смертей
- rounds_per_games - ср. количество сыгранных раундов за игру
- opening_kills_percent - % первых убийств
- opening_deaths_percent - % первых смертей
- kill_assist_per_death - (ср. убийств + ср. ассистов) / ср.смертей
- kill_assist - ср.убийств + ср.ассистов
- headshot_opening_kill_per_round - 1-х убийств за раунд * % хедшотов
- mean_opening_death_per_death - 1-х смертей за раунд / ср. смертей за раунд
- opening_kill_rating_per_rating - рейтинг 1-х килов / рейтинг
- win_after_first_kill_per_opening_deaths - доля выигранных раундов при 1-м убийстве игрока / ср. 1-х смертей за раунд
- mean_kills_assists - ср(ср.убийств, ср. ассистов)
- kast - ср. убийств + ср.ассистов + (1-ср.смертей) + ср.спасение тиммейта
- opening_kill_rating_mtp_opening_kills_perc - рейтинг 1-х убийств * % первых убийств
- opening_kill_rating_mtp_opening_death_perc - рейтинг 1-х убийств * % первых смертей
- opening_kills_per_mean_kills - 1-х убийств / ср. убийств за раунд
- headshot_damage - урон от выстрелов за раунд * хедшоты %
- opening_hedshot_kills - % первых убийств * хедшоты %
- mean_headshot_kills_per_round - ср. убийств за раунд * хедшоты %
- total_win_rounds_with_opening_kills - всего раундов * доля выигранных раундов при 1-м убийстве игрока
- mean_damage_per_kills - ср.дамагза раунд / ср.убийств за раунд

При этом признаким крайне сильно коррелируют между собой, удалил признаки, не имеющие выского показателя anova и имеющие высокую корреляцию с другими признаками

При попытки полностью сократить коррелирующие между собой признаки, резко падало качество модели, поэтому сохранились высококоррелирующие признаки (но только те, которые имеюст высокую оценку anova)

## Модели

Обучил наиболее перспективные модели, подобрал параметры через GridSearch, построил ROC_AUC

RandomForestClassifier:

<img width="352" alt="image" src="https://user-images.githubusercontent.com/73405095/205590772-81a8570a-86cf-435e-bd7b-aa422db3e8d9.png">

SVC:

<img width="229" alt="image" src="https://user-images.githubusercontent.com/73405095/205590951-c4b25c04-5e27-43f2-a529-a54ec1343559.png">

MLPClassifier:

<img width="248" alt="image" src="https://user-images.githubusercontent.com/73405095/205591021-25e93d88-ee6c-483a-b88e-6d0d453047e4.png">

LogisticRegression:

<img width="246" alt="image" src="https://user-images.githubusercontent.com/73405095/205591136-05f06add-cc93-4541-bba6-326753920c0f.png">

Simple NeuralNetwork:

<img width="236" alt="image" src="https://user-images.githubusercontent.com/73405095/205591441-d42ce318-e7bb-45c8-a95d-eb0f92b9386d.png">

Тестовые данные также использовались при обучении (но использовал кросс-валидацию), поэтому метрика высокая

Модели сильно переобучились - это связано с малым количеством примеров для обучения, высокая корреляция признаков также негативно влияет на результат (нужно получить или сгенерировать новые, не коррелирующие с друг другом признаки)

## Что можно улучшить

- получить больше примеров игр
- получить новые данные по играм и сгенерировать, избавиться от высокой корреляции признаков
- попробовать суммировать данные игроков в единые показатели команды
