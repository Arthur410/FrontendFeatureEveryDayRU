# Central workflow vs Git flow vs TBD

Created: October 3, 2024 8:39 PM

## **Central Workflow**

Просто комит и сразу в мастер. А ветки для слабаков.

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/7e6/a18/ad1/7e6a18ad1d174aed3dfb716e235709b6.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/7e6/a18/ad1/7e6a18ad1d174aed3dfb716e235709b6.png)

Цепочка комитов в master-ветку

В итоге Central Workflow подходит:

- Для небольших проектов (pet-проектов).
- Для команд из 2-3 разработчиков.
- Когда мастер сломан.

## **Git Flow**

Процесс — это все! Сложный процесс со сложной моделью ветвления.

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/0dc/e50/cfb/0dce50cfbdd50758e8e046429a77fd8c.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/0dc/e50/cfb/0dce50cfbdd50758e8e046429a77fd8c.png)

Так выглядит этот процесс у автора https://nvie.com/posts/a-successful-git-branching-mode

Чтобы было проще, постараюсь изложить процесс в виде цикла разработки фичи:

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/31c/232/685/31c2326851c08789bfb539d0f8eb43e8.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/31c/232/685/31c2326851c08789bfb539d0f8eb43e8.png)

Исходное состояние

Для создания фичи, вы создаете ветку ***feature***. Ветвите вы её от ветки ***develop:***

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/1be/cbe/fef/1becbefef816ee9453d5a0544fd60cba.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/1be/cbe/fef/1becbefef816ee9453d5a0544fd60cba.png)

Сделали ветку feature, в которой будет вестись разработка

Ветка создана, можно начинать разработку.

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/105/41e/6fc/10541e6fc2ecf4c35cc8cf612a2bc9b8.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/105/41e/6fc/10541e6fc2ecf4c35cc8cf612a2bc9b8.png)

Вы поработали, сделали несколько комитов

Ваша команда долго и упорно готовила фичу — теперь всё готово! Но другие команды разработки тоже делают свои проекты. Допустим, готовые фичи уже слили в ***develop***-ветку, при этом ваша ветка сильно отстала.

**[Первый раз]** Вы решаете конфликты и сливаете ветку в develop:

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/fec/01a/862/fec01a8628f06ef798cd331601d4caef.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/fec/01a/862/fec01a8628f06ef798cd331601d4caef.png)

Время релиза! Вы решили поделится своей фичей с пользователями. Создаётся ветка ***release*** от вашего комита.

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/4fc/c84/44c/4fcc8444c1e46e098f305d43d33435bf.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/4fc/c84/44c/4fcc8444c1e46e098f305d43d33435bf.png)

Но релиз нужно стабилизировать. В нем есть баги, и конфликтующие фичи. Для этого делаем несколько hotfix. Важно, что делаем мы их в релиз-ветке, потому что планируем выкатить именно её.

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/e1f/006/8b1/e1f0068b1cc388b2be74797088924ab4.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/e1f/006/8b1/e1f0068b1cc388b2be74797088924ab4.png)

Релиз стабилизирован. Сливаем его в master c тегом v1.0.0 и катим на prod. Хотфиксы тоже надо слить в develop-ветку. За это время develop опять ушёл вперед.

**[Второй раз]** Вы решаете конфликты и сливаете ветку в develop:

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/885/e02/609/885e02609d7781a2cc8d81d1b9b819b2.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/885/e02/609/885e02609d7781a2cc8d81d1b9b819b2.png)

На prod мы неожиданно ловим еще несколько ошибок, которые важно пофиксить. Вы делаете хотфикс на prod и ветку v1.0.1.

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/713/2c5/0cc/7132c50cc1c8011743efb16669f5ed65.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/713/2c5/0cc/7132c50cc1c8011743efb16669f5ed65.png)

Но новый хотфикс тоже нужно положить обратно в develop-ветку. За это время develop опять ушёл вперед.

**[Третий раз]** Вы решаете конфликты и сливаете ветку в develop:

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/200/74d/e36/20074de36fac62ddffbab154e1bc331c.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/200/74d/e36/20074de36fac62ddffbab154e1bc331c.png)

В итоге Git Flow:

- Очень сложно и запутано.
- Много конфликтов в процессе.
- Долгий релизный цикл, который плохо подходит для CI\CD.
- Подходит для ПО с классическим релейным циклом (параллельная работа над разными версиями, релиз раз в месяц на дискетах).

## **Trunk Based Development**

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/025/94e/5fd/02594e5fde4678af4e5b579f6452aa82.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/025/94e/5fd/02594e5fde4678af4e5b579f6452aa82.png)

trunkbaseddevelopment.com

Характеризуется всего тремя типами веток и итеративным подходом к разработке фичи (привет, Scrum).

Постараюсь изложить его в виде цикла разработки фичи:

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/07d/371/b39/07d371b39c5ebf031f1d7c8844996639.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/07d/371/b39/07d371b39c5ebf031f1d7c8844996639.png)

Исходное состояние

Для создания фичи, вы создаете ветку ***feature***. Ветвите вы её от ветки ***master:***

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/108/866/373/108866373a258062495e9828a0af5f18.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/108/866/373/108866373a258062495e9828a0af5f18.png)

Делаете несколько комитов, заглушки под фичу и необходимые интерфейсы:

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/32e/7b2/5ad/32e7b25ade073ebc989aea73edeac86d.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/32e/7b2/5ad/32e7b25ade073ebc989aea73edeac86d.png)

Сливаете ***фича-ветку*** в ***master***. Ваш функционал может быть закрыт ***feature-toggle*** или `if false { ... }` — «сырой» функционал не должен быть доступен пользователю.

- Важно!
    
    Так как с момента ветвления до момента слияния вашей фича-ветки прошло очень мало времени, master не успеет далеко уйти и у вас будет минимум или полное отсутствие конфликтов при слиянии. При этом вы «застолбили» для своей фичи место, подсветили другим командам, к примеру, интерфейсы, которые планируете имплементить. И другие команды будут сразу учитывать это в своей работе.
    
    А ещё у вас очень маленькие PR\MR, которым можно легко и быстро провести code-review. Знаете же эту классическую проблему — большие PR смотрят долго и некачественно.
    

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/299/6f7/f74/2996f7f74e5ceb163a5136447079a1fa.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/299/6f7/f74/2996f7f74e5ceb163a5136447079a1fa.png)

У вас может быть организован процесс непрерывного деплоя, или деплоя по расписанию, в prod. Так как в master не сливают сложные и плохо протестированные изменения — master остаётся чистым, готовым к релизам.

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/7e3/b16/2fc/7e3b162fc54cebd80fd8c38e6411b164.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/7e3/b16/2fc/7e3b162fc54cebd80fd8c38e6411b164.png)

Вы продолжаете работать над своей фичей в новой итерации — продолжаете её и углубляете.

Важно, чтобы ваша новая фича-ветка оказалось очень короткоживущей, чтобы ловить минимум конфликтов при слиянии и углубить создание фичи.

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/d74/9e5/062/d749e50628591f633a17d4c485970123.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/d74/9e5/062/d749e50628591f633a17d4c485970123.png)

### **В итоге, TBD — это:**

- Ветки, которые живут несколько дней.
- Feature-flags для закрытия «сырого» функционала.
- Continuous Сode Review. Код-ревью фича проходит каждую итерацию — дифф маленький, ревью делается быстро.
- Чистый мастер, готовый к релизу в любой момент.

### **Плюсы TBD:**

- Минимум конфликтов при разработке.
- Готовность к релизам в любой момент без подготовки.
- Очень быстрая и качественная обратная связь на PR\MR.

### **Минусы TBD:**

- Не подходит, если в команде много junior-разработчиков. Ребятам гораздо легче делать\ревьювить готовую фичу целиком, чем разделить работу на этапы и держать прошлые итерации в голове.
- Нужно доверять комитерам, иначе будут висеть недоделанные фичи и будет непонятно, кто и когда их доделает.
- Если потребуется revert — будет больно. Мастер постоянно бежит вперед и revert функционал спустя пару дней уже тяжело. Проще исправить или удалить руками.

### **Кому подходит TBD:**

- Опытным командам с небольшим количеством junior-разработчиков.
- Большим командам, которые параллельно работают над большим количеством фич.
- Приложениям с долгим циклом регрессивного тестирования.
- Проектам с непрерывным релизным циклом.

### **Для кого TBD не очень хорошо подходит:**

- Junior-командам.
- OpenSource-проектам.
- Проектам с жестким (классическим) релизным циклом с отгрузкой софта клиентам на дискетах.

## **Почему всё же TBD**

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/6b6/aab/113/6b6aab11343c266b315c3a4caed0c17a.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/6b6/aab/113/6b6aab11343c266b315c3a4caed0c17a.png)

### **Time To Market**

Использование такого подхода дает огромный прирост к TTM метрике.

Короткоживущие ветки позволяют вам экономить время на конфликты при слиянии, а когда нет конфликтов — и нервы экономишь. Багов после решения конфликтов возникает гораздо меньше.

Фича-тогглы, которыми закрываете не протестированный функционал позволяют вам увереннее катиться в прод, потому что фичу сразу можно отключить. И времени на стабилизацию релиза тратится меньше — все новые фичи изначально под тогглом.

Чистый мастер позволяет вам делать релизы настолько часто, насколько нужно. Релизы перестают быть переполнены фичами, а ещё пропадают «ждуны», которые просят задержать релиз.

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/e29/03b/f6d/e2903bf6db77f3ed4d54e24fad2a733b.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/e29/03b/f6d/e2903bf6db77f3ed4d54e24fad2a733b.png)

Раньше при релизе в чатах постоянно такое мелькало

### **Релизы бэкенда-монолита:**

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/d2e/895/e80/d2e895e80baaffed73ddd33518bbe2b3.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/d2e/895/e80/d2e895e80baaffed73ddd33518bbe2b3.png)

### **Релизы приложений:**

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/908/941/73b/90894173bb68f561c905c13aac89a32f.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/908/941/73b/90894173bb68f561c905c13aac89a32f.png)

В таблицах примеры того, как ускорились релизы в Авито за несколько лет. Цифры не самые свежие, но честные.
