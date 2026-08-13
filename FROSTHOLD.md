# Frosthold — правила работы с кодом

Frosthold — форк [crystall-edge](https://github.com/crystallpunk-14/crystall-edge),
который сам является форком [CrystallPunk 14](https://github.com/crystallpunk-14/crystall-punk-14),
который форк [Space Station 14](https://github.com/space-wizards/space-station-14).

Мы находимся в самом низу этой цепочки и регулярно подтягиваем изменения сверху.
Всё, что написано ниже, существует ради одной цели: **чтобы обновление из апстрима
не превращалось в разбор сотен конфликтов.**

## Правило первое: свой код — в свои файлы

Git сравнивает файлы построчно. Если мы правили тот же файл, что и апстрим, —
получаем конфликт и разбираем его руками. Если файл трогали только мы — конфликта нет.

Поэтому весь новый контент кладём в папки с префиксом `_Frosthold`:

```
Content.Server/_Frosthold/          серверная логика
Content.Client/_Frosthold/          клиентская логика (UI, отрисовка)
Content.Shared/_Frosthold/          общий код: компоненты, события, системы
Resources/Prototypes/_Frosthold/    предметы, существа, рецепты (YAML)
Resources/Textures/_Frosthold/      спрайты (.rsi)
Resources/Audio/_Frosthold/         звуки
Resources/Locale/ru-RU/_Frosthold/  русские тексты
Resources/Locale/en-US/_Frosthold/  английские тексты
Resources/Maps/_Frosthold/          карты
```

Такая же система у наших предков: `_CE` — crystall-edge, `_CP14` — CrystallPunk.
Не трогайте их папки, они обновляются сверху.

## Правило второе: если всё-таки лезем в чужой файл — помечаем

Иногда без этого никак. Например, чтобы добавить свою расу, надо вписать её
в существующий список. В таком случае обрамляем правку маркерами:

```csharp
// Frosthold-Start
ваш код
// Frosthold-End
```

```yaml
# Frosthold-Start
ваш YAML
# Frosthold-End
```

При конфликте сразу видно, где наше, а где апстримовое. Без маркеров через полгода
никто не вспомнит, зачем эта строка и можно ли её выкинуть.

## Правило третье: баги апстрима чиним апстриму

Если нашли баг, который есть и в crystall-edge, — почините у себя и отправьте им
пул-реквест. Иначе будете таскать эту заплатку через каждое обновление вечно.

## Соглашение об именах

Классы, компоненты и системы, специфичные для нас, называем с префиксом `FH`:

```csharp
public sealed partial class FHRitualComponent : Component
public sealed class FHRitualSystem : EntitySystem
```

Прототипы в YAML — с префиксом `FH` в `id`:

```yaml
- type: entity
  id: FHRitualDagger
  parent: BaseItem
```

Так в логах и в отладке сразу видно, чей это код.

## Как обновиться из апстрима

```bash
git remote add upstream https://github.com/crystallpunk-14/crystall-edge.git
git fetch upstream
git merge upstream/master
```

Первый раз лучше делать это вместе и не в конце дня.
