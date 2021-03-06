# Управление загруженными изображениями
Убедитесь, что вы реализовали базовый класс модального окна `BaseModal` из [предыдущего пункта](./upload_images.md).

## Класс PreviewModal
Должен наследоваться от класса `BaseModal` и иметь следующий функционал:

1. Конструктор, в котором необходимо:
    1. Вызвать конструктор родителя для предварительных настроек.
    2. Выполнить регистрацию событий с помощью метода `registerEvents`.

2. Метод `registerEvents`, который выполняет подписки на события кликов по элементам:
    1. Клик по элементу крестика (в заголовке модалки) должен закрывать модальное окно.
    2. Так как мы изначально не знаем сколько элементов изображений будет отображаться в модалке (сколько полей ввода и кнопок будет), то добавим обработчик события на весь блок тела модалки (с классом `content`). А внутри обработчка клика будем конкретно проверять, на каком элементе был клик:
        1. Если клик был на элементе с классом `delete`, то необходимо удалять изображения с диска. Для этого:
            * В элемент иконки (`i`) присвойте следующий набор классов: `'icon spinner loading'`.
            * Заблокируйте кнопку удаления, добавив класс `disabled`.
            * Выполните запрос на удаление файла (с помощью метода `Yandex.removeFile`).
            * После выполнения запроса (в колбеке), в случае успешного запроса (если в ответе будет значение `null`), удаляйте весь блок с информацией об изображении.
        2. Если клик был на элементе с классом `download`, то необходимо скачать файл на компьютер. Для этого воспользуйтесь методом `Yandex.downloadFileByUrl`.

3. Метод `showImages` получает все изображения, меняет порядок на противоположный (с помощью метода `reverse`). Для каждого изображения получает блок контейнер (с изображением, полем ввода и кнопкной загрузки). Объединяет все полученные разметки (с помощью метода `join`) и сохраняет полученую разметку (из блоков контейнеров ко всем изображениям) в свойство `innerHTML` содержимого модального окна (блока `content`).

4. Метод `formatDate` преобразует полученную дату от Yandex API (ISO) в удобный для отображения формат: `30 декабря 2021 г. в 23:40`

5. Метод `getImageInfo` возвращает строку формата:
```javascript
<div class="image-preview-container">
  <img src='XXX' />
  <table class="ui celled table">
  <thead>
    <tr><th>Имя</th><th>Создано</th><th>Размер</th></tr>
  </thead>
  <tbody>
    <tr><td>AAA</td><td>BBB</td><td>CCCКб</td></tr>
  </tbody>
  </table>
  <div class="buttons-wrapper">
    <button class="ui labeled icon red basic button delete" data-path='PPP'>
      Удалить
      <i class="trash icon"></i>
    </button>
    <button class="ui labeled icon violet basic button download" data-file='FFF'>
      Скачать
      <i class="download icon"></i>
    </button>
  </div>
</div>
```
где:
* `XXX` путь к изображению
* `AAA` имя изображения
* `BBB` форматированная дата создания файла (форматирование происходит с помощью метода `formatDate`)
* `CCC` размер файла (в Кб)
* `PPP` путь к изображению относительно ЯДиска
* `FFF` ссылка на файл

![](./img/PreviewModal.png)