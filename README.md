<!DOCTYPE html>
<html lang="ru"><head><meta charset="utf-8"><title>Shop Simulator</title>
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover, maximum-scale=1, user-scalable=no">
<style>
:root{box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px);--bg:#f3f1ec;--fg:#222;--btn:#fff}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#1b1c20;--fg:#eee;--btn:#2c2e35}}
:root[data-theme="dark"]{--bg:#1b1c20;--fg:#eee;--btn:#2c2e35}
html,body{margin:0;background:var(--bg);color:var(--fg);font-family:system-ui,Arial,sans-serif}
body{display:flex;flex-direction:column;align-items:center;padding:8px}
h1{margin:6px 0;font-size:22px}
canvas#c{max-width:100%;height:auto;border-radius:8px;background:#87c4e8}
.ctl{display:flex;flex-wrap:wrap;gap:6px;justify-content:center;margin-top:8px}
button{font-size:16px;padding:9px 13px;border:1px solid #888;border-radius:8px;background:var(--btn);color:var(--fg);touch-action:manipulation;cursor:pointer}
p{font-size:13px;margin:6px;text-align:center;opacity:.8}
#menu{position:fixed;inset:0;background:rgba(0,0,0,.65);display:flex;align-items:center;justify-content:center;z-index:5;padding:12px}
#menu .card{max-height:92vh;overflow:auto;background:var(--bg);border-radius:14px;padding:20px;max-width:420px;width:100%;text-align:center}
#menu h2{margin:0 0 4px;font-size:30px}
.row{display:flex;flex-wrap:wrap;gap:6px;justify-content:center;margin:8px 0}
.sw{width:32px;height:32px;padding:0;border-radius:6px;border:2px solid #888}
.sw.on,.eb.on,#mt.on,#gt.on{outline:3px solid #3a86ff}
#start{font-size:20px;padding:12px 34px;background:#3a86ff;color:#fff;border-color:#3a86ff;margin-top:10px}
input{font-size:16px;padding:10px;border-radius:8px;border:1px solid #888;width:100%;box-sizing:border-box;margin:5px 0;background:var(--btn);color:var(--fg)}
#how .card{background:var(--bg);border-radius:14px;padding:20px;max-width:360px;width:100%;text-align:center}
#how h3{margin:0 0 8px;font-size:22px}
#how button{margin:4px 2px}
#wrap{position:relative;display:inline-block;max-width:100%;line-height:0}
canvas#c{touch-action:none}
.arr{position:absolute;bottom:14px;width:43px;height:32px;background:rgba(255,255,255,.28);border:1px solid rgba(255,255,255,.55);border-radius:6px;color:rgba(255,255,255,.9);font-size:14px;display:flex;align-items:center;justify-content:center;touch-action:none;user-select:none;-webkit-user-select:none;line-height:1;cursor:pointer}
#how .card{max-width:480px;text-align:left;max-height:88vh;overflow:auto}
#how h4{margin:12px 0 4px;font-size:15px}#how p,#how li{font-size:13px;margin:3px 0}#how ul{margin:4px 0;padding-left:18px}
.arr.edit{outline:2px dashed #fff;background:rgba(255,255,255,.5);cursor:move}
#achm .card,#cred .card,#altm .card,#setm .card,#hwCh .card,#debCh .card,#rivalIntro .card{background:var(--bg);border-radius:14px;padding:18px;max-width:440px;width:100%;max-height:88vh;overflow:auto}
#achm h3,#cred h3,#altm h3,#setm h3,#hwCh h3,#debCh h3,#rivalIntro h3{margin:0 0 6px;text-align:center;font-size:22px}#achm h4{margin:12px 0 4px;font-size:15px}
.ctl button{touch-action:none}
</style></head><body>
<h1>Shop Simulator</h1>
<div id="wrap"><canvas id="c" width="900" height="420"></canvas><div class="arr" id="tl" style="left:10px">◀</div><div class="arr" id="tr" style="left:61px">▶</div><div class="arr" id="tj" style="display:none">⤒</div></div>
<button id="rv" style="display:none;font-size:20px;padding:12px 30px;background:#52b788;color:#fff;border-color:#52b788;margin-top:8px">✨ Возродиться — $500</button><button id="rs" style="display:none;font-size:18px;padding:10px 24px;margin-top:8px;margin-left:6px">🔄 Ресет (R)</button>
<div class="ctl">

<button data-k="1">🍎1</button><button data-k="2">🥕2</button><button data-k="3">🥛3</button><button data-k="4">🧀4</button><button data-k="5">🗡5</button><button data-k="6">📢6</button><button data-k="7">👟7</button><button data-k="8">⭐8</button><button data-k="9">🏁9</button><button id="be">E</button><button id="bf" style="display:none">🤢 F</button><button id="bx">✖ Отказать</button><button id="bm">☰ Меню</button>
</div>
<p>← → / A D — ходить, ↑ / W / Пробел — прыжок. 1–5 — купить (🍎 🥕 🥛 🧀 🗡; в правом магазине дешевле), 6 — реклама, 7 — скорость, 8 — +0.5 звезды, 9 — закончить игру (нужно 5 звёзд и $10000); 6–9 только в правом магазине. Левый магазин: E — продать, Q — отказать (3 отказа = −0.5★). Кликер (розовая комната за казино): прыгай и бей блок снизу, 10 ударов = $1. Дом магии (за кликером): E — случайное зелье за $100, 1 — выбрать эффект за $1000. Казино: 1/2/3 — ставка +10/+100/+1000, 6 — +1, 4 — всё, 5 — сброс, E — крутить. Проигрыш: 0 звёзд.</p>
<div id="menu"><div style="position:absolute;right:12px;bottom:8px;color:#fff;opacity:.85;font-size:13px">Версия: 1.0</div><div class="card">
<h2>Shop Simulator</h2>
<canvas id="pv" width="96" height="96"></canvas>
<div><b>Цвет</b></div><div class="row" id="cols"></div>
<div class="row"><button id="gt"></button></div><div class="row" id="cols2"></div>
<div><b>Глаза</b></div><div class="row" id="eyes"></div>
<div class="row"><button id="mt"></button></div>
<div id="lock" style="color:#e63946;min-height:18px;font-size:13px;font-weight:bold"></div>
<button id="start">▶ Обычная игра</button>
<div id="nrec" style="font-size:13px;opacity:.85;margin-top:4px"></div>
<div class="row"><button id="nm" style="display:none">↻ Обычная игра</button></div>
<div class="row"><button id="altBtn" style="font-weight:bold">🎭 Альтернативные режимы</button></div>
<div style="border-top:1px solid #8886;margin-top:10px;padding-top:6px;font-size:12px;opacity:.7">Справка и сервис</div>
<div class="row"><button id="achBtn">🏆 Достижения</button></div>
<div class="row"><button id="setBtn">⚙ Настройки</button></div>
<div class="row"><button id="howBtn">📖 Как играть</button></div>
<div style="border-top:1px solid #8886;margin-top:10px;padding-top:6px;font-size:12px;opacity:.7">О проекте</div>
<div class="row"><button id="credBtn">👥 Над игрой работали</button></div>
<div class="row"><a id="tgLink" href="https://t.me/Shop_Simulator" target="_blank" rel="noopener noreferrer" style="display:inline-block;font-size:16px;padding:9px 13px;border:1px solid #888;border-radius:8px;background:var(--btn);color:var(--fg);text-decoration:none;cursor:pointer">✈ Телеграм канал</a></div>
</div></div>
<div id="how" style="display:none;position:fixed;inset:0;background:rgba(0,0,0,.7);z-index:7;align-items:center;justify-content:center;padding:12px"><div class="card">
<h3 style="text-align:center">📖 Как играть</h3>
<div id="howBody"><h4>🎯 Цель</h4><p>Зарабатывай деньги, доведи магазин до <b>5 звёзд</b>, накопи <b>$10000</b> и нажми <b>9</b> в правом магазине («Закончить игру»). Эти $10000 при этом списываются.</p>
<h4>🎮 Управление</h4><ul>
<li><b>Компьютер:</b> ← → или A D — ходить, ↑ / W / Пробел — прыжок.</li>
<li><b>Телефон:</b> белые стрелки ◀ ▶ на экране — ходить, нажатие на любое другое место экрана — прыжок. Во время ходьбы можно нажимать другие кнопки — игрок не остановится. Размер, прозрачность и положение кнопок, отдельную кнопку прыжка, всплывающие окна у магазинов, и старую графику можно менять в меню «⚙ Настройки».</li>
<li><b>E</b> — действие (продать, крутить барабан, сварить зелье), <b>Q</b> — отказать покупателю, <b>1–9</b> — покупки и выбор, <b>Esc</b> — меню, <b>R</b> — начать заново после проигрыша, <b>V</b> — возродиться (только в Детском режиме). Все эти действия есть и в виде кнопок под экраном.</li></ul>
<h4>🛒 Как зарабатывать</h4><ul>
<li><b>Правый магазин «Покупка»:</b> клавиши 1–5 покупают яблоко, морковку, молоко, сыр и меч. Вне магазина товары можно купить дороже (кроме Hard Mode).</li>
<li><b>Левый магазин «Продажа»:</b> когда приходит покупатель, смотри его заказ над магазином. Зайди внутрь и нажми <b>E</b>, чтобы продать. Покупатели приходят, только если у тебя есть товар.</li>
<li>Если покупатель ушёл, не дождавшись, магазин теряет 0,5 звезды. Три отказа (<b>Q</b>) тоже отнимают 0,5 звезды. Остались 0 звёзд — игра проиграна.</li>
<li>Чем больше звёзд, тем больше чаевых. Звёзды можно купить в правом магазине (<b>8</b>).</li></ul>
<h4>🏪 Правый магазин</h4><ul><li><b>6</b> — реклама: покупатели приходят чаще. <b>7</b> — скорость игрока. <b>8</b> — +0,5 звезды. <b>9</b> — закончить игру (нужны 5★ и $10000).</li></ul>
<h4>🎰 Казино (за правым магазином)</h4><ul><li>1 / 2 / 3 — ставка +$10 / +$100 / +$1000, 6 — +$1, 4 — вся сумма, 5 — сброс, <b>E</b> — крутить. Выпадет X-2 (теряешь ставку) или X2 (получаешь ставку). Ставка не больше баланса.</li></ul>
<h4>🟪 Кликер</h4><ul><li>Прыгай и бей блок снизу: за каждые 10 ударов дают $1.</li></ul>
<h4>🧙 Дом магии</h4><ul><li><b>E</b> — случайное зелье за $100, <b>1</b> — выбрать любой эффект за $1000. Эффекты действуют 1 минуту: увеличение, уменьшение, скорость, прибыль, анти-прибыль, удача (шансы на хорошее ×2) и неудача (шансы на плохое ×2). Зелье смерти сразу заканчивает игру!</li></ul>
<h4>💀 Проигрыш</h4><ul><li>Возродиться можно только в Детском режиме (бесплатно): <b>V</b> или кнопка под экраном. В остальных режимах после проигрыша нажми <b>R</b>, чтобы начать заново.</li></ul>
<h4>🔥 Hard Mode</h4><ul><li>25 минут на прохождение, покупатели ждут 1 секунду, цены выше, возрождения нет. Подробности — в «🎭 Альтернативные режимы». Награда — лицо Демона.</li></ul>
<h4>🎃 Хеллоуинский режим</h4><ul><li>Вместо денег конфеты, каждую минуту — случайный эффект зелья. Перед стартом выбираешь: без зелья смерти или с ним. Подробности — в «🎭 Альтернативные режимы».</li></ul>
<h4>🧒 Детский режим</h4><ul><li>Всё стоит 1 деньгу, клиенты ждут вечно, в казино только выигрыш. Подробности — в «🎭 Альтернативные режимы».</li></ul>
<h4>🌀 Режим с дебафами</h4><ul><li>Перед стартом выбираешь: 3 случайных дебафа каждую минуту или 10 случайных дебафов на всю игру. Нарушишь хоть один — проигрыш без возрождения. Подробности — в «🎭 Альтернативные режимы».</li></ul>
<h4>🏪 Уничтожь конкурентов</h4><ul><li>Рейтинг магазина вместо звёзд: доведи его от 500 до 0 (портя товар, отказывая клиентам или взорвав магазин за $20000). Подробности — в «🎭 Альтернативные режимы».</li></ul>
<h4>🎭 Лица</h4><ul><li>Часть лиц открывается за достижения (кнопка «🏆 Достижения» в меню). Выбранный скин и достижения сохраняются в этом браузере. Градиент открывается за достижение «Спасибо за поддержку» (нужно нажать «✈ Телеграм канал»), а альтернативные режимы — после прохождения обычного режима.</li></ul>
</div>
<div style="text-align:center;margin-top:10px"><button id="howX">Закрыть</button></div>
</div></div>
<div id="altm" style="display:none;position:fixed;inset:0;background:rgba(0,0,0,.7);z-index:7;align-items:center;justify-content:center;padding:12px"><div class="card">
<h3>🎭 Альтернативные режимы</h3><div style="text-align:center;font-size:13px;opacity:.8">Выбери режим — начнётся новая игра</div>
<div class="row"><button id="hm" style="background:#d62828;color:#fff;border-color:#d62828;font-weight:bold">🔥 Hard Mode</button><button id="hi" title="Что меняется в Hard Mode">ℹ</button></div>
<div id="hinfo" style="display:none;text-align:left;font-size:13px"><b>🔥 Hard Mode — что меняется:</b><div id="hrec" style="margin:4px 0;font-weight:bold"></div><ul id="hinfoL" style="margin:6px 0;padding-left:18px">
<li>Новая игра, на прохождение только <b>25 минут</b> (время вышло — проигрыш).</li>
<li>Цены в магазине как «с доставкой» ($8 / $55 / $110 / $550 / $1100 за меч), а доставки нет — товары только в правом магазине.</li>
<li>Покупатели ждут всего <b>1 секунду</b>. Чаевых нет.</li>
<li>Скорость стоит в 2 раза дороже. Реклама, звёзды и завершение режима ($10000 и 5 звёзд) — как в обычном режиме.</li>
<li><b>Один</b> отказ клиенту или один необслуженный клиент — все звёзды пропадают, игра окончена.</li>
<li>Казино: шанс проиграть в 2 раза больше (X-2 — 2/3, X2 — 1/3).</li>
<li>Кликер: $1 за 50 ударов вместо 10.</li>
<li>Дом магии: случайное зелье стоит $1000, выбрать зелье нельзя. Шанс смерти ×3, уменьшения и анти-прибыли ×2.</li>
<li>Возродиться нельзя — после смерти всё сначала.</li>
<li>Награда: лицо <b style="color:#ff2a2a">Демона</b>.</li></ul></div>
<div class="row"><button id="hwBtn" style="background:#7b2fd0;color:#fff;border-color:#7b2fd0;font-weight:bold">🎃 Хеллоуинский режим</button><button id="hwInfoBtn" title="Что меняется в Хеллоуинском режиме">ℹ</button></div>
<div id="hwInfo" style="display:none;text-align:left;font-size:13px"><b style="color:#b46bff">🎃 Хеллоуинский режим — что меняется:</b><div id="wrec" style="margin:4px 0;font-weight:bold"></div><ul id="hwInfoL" style="margin:6px 0;padding-left:18px">
<li>Вместо денег — <b>конфеты 🍬</b>. Цены и цель те же, что в обычном режиме.</li>
<li>Перед стартом выбираешь: <b>без зелья смерти</b> или <b>с зельем смерти</b> (оно может выпасть случайно).</li>
<li>Каждую минуту ты получаешь <b>случайный эффект зелья</b> (зелье смерти — только если выбран вариант «с зельем смерти»).</li>
<li>В доме магии можно ускорять получение эффектов: ур.1 — 30 с, ур.2 — 15 с, ур.3 — 10 с, ур.4 — 5 с, ур.5 — 1 с (клавиша 2).</li>
<li>Случайное зелье купить нельзя — только выбрать определённое за 100 конфет (клавиша 1).</li>
<li>Награды: достижения «Хеллоуинский режим» (лицо Волшебник) и «Настоящая ведьма» — пройти с зельем смерти (лицо Ведьма).</li></ul></div>
<div class="row"><button id="kidBtn" style="background:#4fc3f7;color:#00334d;border-color:#4fc3f7;font-weight:bold">🧒 Детский режим</button><button id="kidInfoBtn" title="Что меняется в Детском режиме">ℹ</button></div>
<div id="kidInfo" style="display:none;text-align:left;font-size:13px"><b style="color:#2a9fd8">🧒 Детский режим — что меняется:</b><div id="krec" style="margin:4px 0;font-weight:bold"></div><ul id="kidInfoL" style="margin:6px 0;padding-left:18px">
<li>Сразу есть все <b>5 звёзд</b>, скорость и реклама на максимальном уровне.</li>
<li>Всё, кроме завершения режима, стоит <b>1 деньгу</b>. Завершение — как в обычном режиме ($10000 и 5 звёзд).</li>
<li>Клиенты ждут вечно, отказывать можно сколько угодно — звёзды не снимаются.</li>
<li>Казино: можно только выиграть. Кликер: $1 за 1 удар.</li>
<li>Дом магии: случайные зелья только хорошие.</li>
<li>Возрождение после смерти бесплатное.</li>
<li>Награды: достижения «Детский режим» (лицо Младенец) и «Даже тут ты умер» (лицо Грустный).</li></ul></div>
<div class="row"><button id="debBtn" style="background:#0d9488;color:#fff;border-color:#0d9488;font-weight:bold">🌀 Режим с дебафами</button><button id="debInfoBtn" title="Что меняется в режиме с дебафами">ℹ</button></div>
<div id="debInfo" style="display:none;text-align:left;font-size:13px"><b style="color:#14b8a6">🌀 Режим с дебафами — что меняется:</b><div id="drec" style="margin:4px 0;font-weight:bold"></div><div id="erec" style="margin:4px 0;font-weight:bold"></div><ul id="debInfoL" style="margin:6px 0;padding-left:18px">
<li>Перед стартом выбираешь: <b>3 случайных дебафа каждую минуту</b> (с самого начала игры) или <b>10 случайных дебафов на всю игру</b>.</li>
<li>Дебафы в наборе не повторяются. Нарушил хотя бы один — <b>игра проиграна</b>, возрождения нет.</li>
<li>Дебаф «❓ Неизвестный» скрывает своё правило: это может быть любой другой дебаф. Когда он закончится (или игра закончится), ты узнаешь, какой именно.</li>
<li>Казино, кликер и дом магии тоже считаются магазинами (для дебафа «Боязнь магазинов»).</li>
<li>Цель и цены как в обычном режиме.</li>
<li>Награды: «Режим с дебафами с разнообразием» (3 в минуту, лицо Слеер) и «Режим с дебафами в 10'ти кратном размере» (10 на всю игру, лицо Череп).</li></ul>
<b>Все дебафы:</b><ol id="debAll" style="margin:4px 0;padding-left:22px;font-size:12px"></ol></div>
<div class="row"><button id="rivalBtn" style="background:#b45309;color:#fff;border-color:#b45309;font-weight:bold">🏪 Уничтожь конкурентов</button><button id="rivalInfoBtn" title="Что меняется в режиме «Уничтожь конкурентов»">ℹ</button></div>
<div id="rivalInfo" style="display:none;text-align:left;font-size:13px"><b style="color:#d97706">🏪 Уничтожь конкурентов — что меняется:</b><div id="rrec" style="margin:4px 0;font-weight:bold"></div><ul id="rivalInfoL" style="margin:6px 0;padding-left:18px">
<li>По сюжету владелец соседнего магазина (для тебя он конкурент) уезжает и просит тебя поработать в его магазине. А ты решил его обанкротить!</li>
<li>Вместо звёзд — <b>рейтинг магазина</b> (0–1000), старт — 500. Рейтинг <b>0</b> — ты выиграл, <b>1000</b> — проиграл.</li>
<li>Рейтинг сам растёт на <b>1 каждую секунду</b>, а обычные продажи его не меняют.</li>
<li>В правом магазине клавиша <b>8</b> включает режим порчи: клавишами 1–5 можно испортить купленный товар за 25% его цены. Испорченный товар продаётся клавишей <b>F</b> за 50% цены, но рейтинг падает (яблоко −1, морковка −5, молоко −10, сыр −25, меч −50).</li>
<li>Отказ клиенту — рейтинг −1, необслуженный клиент — рейтинг −25.</li>
<li>За $20000 можно купить <b>бомбу</b> (клавиша 9 в правом магазине) и взорвать магазин — режим пройден.</li>
<li>Звёзды и обычное завершение режима недоступны. Возрождения нет.</li>
<li>Награды: пять достижений за разные способы закончить режим (кнопка «🏆 Достижения»).</li></ul></div>
<div style="text-align:center;margin-top:8px"><button id="altX">Закрыть</button></div>
</div></div>
<div id="hwCh" style="display:none;position:fixed;inset:0;background:rgba(0,0,0,.7);z-index:7;align-items:center;justify-content:center;padding:12px"><div class="card" style="text-align:center"><h3>🎃 Хеллоуинский режим</h3><div style="font-size:13px;opacity:.8">Выбери вариант:</div>
<div class="row"><button id="hwNo" style="background:#7b2fd0;color:#fff;border-color:#7b2fd0;font-weight:bold">Без зелья смерти</button></div>
<div class="row"><button id="hwYes" style="background:#3a1a66;color:#fff;border-color:#3a1a66;font-weight:bold">С зельем смерти ☠</button></div>
<button id="hwChX">Назад</button></div></div>
<div id="debCh" style="display:none;position:fixed;inset:0;background:rgba(0,0,0,.7);z-index:7;align-items:center;justify-content:center;padding:12px"><div class="card" style="text-align:center"><h3>🌀 Режим с дебафами</h3><div style="font-size:13px;opacity:.8">Выбери вариант:</div>
<div class="row"><button id="debBtn1" style="background:#0d9488;color:#fff;border-color:#0d9488;font-weight:bold">🕐 3 случайных дебафа каждую минуту</button></div>
<div class="row"><button id="debBtn2" style="background:#0f766e;color:#fff;border-color:#0f766e;font-weight:bold">🔟 10 случайных дебафов на всю игру</button></div>
<button id="debChX">Назад</button></div></div>
<div id="setm" style="display:none;position:fixed;inset:0;background:rgba(0,0,0,.7);z-index:7;align-items:center;justify-content:center;padding:12px"><div class="card" style="text-align:center"><h3>⚙ Настройки</h3>
<div class="row"><button id="ctlBtn">🎮 Изменить движение</button></div>
<div class="row"><button id="popBtn"></button></div>
<div class="row"><button id="setG"></button></div>
<button id="setX">Закрыть</button></div></div>
<div id="rivalIntro" style="display:none;position:fixed;inset:0;background:rgba(0,0,0,.7);z-index:7;align-items:center;justify-content:center;padding:12px"><div class="card" style="text-align:center"><h3>🏪 Уничтожь конкурентов</h3>
<p style="text-align:left;font-size:14px">Владелец магазина напротив — твой главный конкурент — уезжает по делам и просит тебя присмотреть за его магазином, пока его нет.</p>
<p style="text-align:left;font-size:14px">Он доверяет тебе, а зря: ты решил воспользоваться шансом и <b>обанкротить</b> его бизнес. Рейтинг магазина сейчас <b>500</b> из 1000. Доведи его до <b>0</b> — и конкурент разорён. Но если рейтинг дойдёт до <b>1000</b>, ты слишком хорошо работал, и магазин процветает.</p>
<div class="row"><button id="rivalGo" style="background:#b45309;color:#fff;border-color:#b45309;font-weight:bold;font-size:18px;padding:10px 24px">Приступить к работе</button></div>
<button id="rivalX">Назад</button></div></div>
<div id="achBar" style="display:none;position:fixed;top:calc(env(safe-area-inset-top,0px) + 8px);left:50%;transform:translateX(-50%) translateY(-20px);opacity:0;transition:opacity .35s,transform .35s;z-index:9;background:rgba(0,0,0,.9);color:#fff;padding:8px 14px;border-radius:12px;gap:10px;align-items:center;max-width:94vw;box-shadow:0 2px 12px rgba(0,0,0,.5);pointer-events:none"></div>
<div id="achm" style="display:none;position:fixed;inset:0;background:rgba(0,0,0,.7);z-index:7;align-items:center;justify-content:center;padding:12px"><div class="card">
<h3>🏆 Достижения</h3><div id="achCnt" style="text-align:center;font-size:13px;opacity:.8"></div>
<div id="achList"></div>
<div style="text-align:center;margin-top:10px"><button id="achX">Закрыть</button></div>
</div></div>
<div id="cred" style="display:none;position:fixed;inset:0;background:rgba(0,0,0,.7);z-index:7;align-items:center;justify-content:center;padding:12px"><div class="card" style="text-align:center">
<h3>👥 Над игрой работали</h3>
<p style="font-size:16px;margin:12px 0"><b>Создатель:</b> KpunepTV55555</p>
<p style="font-size:16px;margin:12px 0"><b>Бета-тестеры:</b> 22GD, TonVon</p>
<p style="font-size:16px;margin:12px 0"><b>Помогали с лицами:</b> 22GD (22GD), kofoch (Слеер)</p>
<button id="credX">Закрыть</button>
</div></div>
<div id="ctlBar" style="display:none;position:fixed;top:8px;left:50%;transform:translateX(-50%);z-index:8;background:rgba(0,0,0,.85);color:#fff;padding:8px 12px;border-radius:12px;font-size:13px;align-items:center;gap:8px;flex-wrap:wrap;justify-content:center;max-width:96vw">
<label style="display:flex;align-items:center;gap:6px"><span>Размер ◀ ▶</span><input id="ctlR" type="range" min="0.5" max="3" step="0.1" value="1" style="width:100px;margin:0"><b id="ctlLab" style="min-width:34px;text-align:right">×1.0</b></label><label style="display:flex;align-items:center;gap:6px"><span>Прозрачность ◀ ▶</span><input id="ctlT" type="range" min="0" max="100" step="5" value="0" style="width:100px;margin:0"><b id="ctlTL" style="min-width:34px;text-align:right">0%</b></label><label style="display:flex;align-items:center;gap:6px"><span>Размер прыжка</span><input id="ctlRJ" type="range" min="0.5" max="3" step="0.1" value="1" style="width:100px;margin:0"><b id="ctlLabJ" style="min-width:34px;text-align:right">×1.0</b></label><label style="display:flex;align-items:center;gap:6px"><span>Прозрачность прыжка</span><input id="ctlTJ" type="range" min="0" max="100" step="5" value="0" style="width:100px;margin:0"><b id="ctlTLJ" style="min-width:34px;text-align:right">0%</b></label>
<button id="ctlJ">Прыжок: весь экран</button><button id="ctlReset">↺ По умолчанию</button><button id="ctlOk" style="background:#3a86ff;color:#fff;border-color:#3a86ff">✔ Готово</button>
<div style="width:100%;text-align:center;opacity:.8">Перетащи кнопки в нужное место</div></div>
<script>
const cv=document.getElementById('c'),x=cv.getContext('2d'),G=360;
const items=[
 {n:'Яблоко',buy:5,out:8,sell:10,c:'#d62828'},
 {n:'Морковка',buy:50,out:55,sell:60,c:'#f77f00'},
 {n:'Молоко',buy:100,out:110,sell:125,c:'#dff3ff'},
 {n:'Сыр',buy:500,out:550,sell:600,c:'#ffd23f'},
 {n:'Меч',buy:1000,out:1100,sell:1250,c:'#b0c4de'}];
const GOAL=10000,SPD=[50,150,400,1000,2500],ADS=[20,50,100,200,400,800,1500,3000,6000,8000],GAP=[5,4.5,4,3.5,3,2.5,2,1.5,1,0.5,0];let PAT=20,hard=false,kid=false;
const inv=[0,0,0,0,0],bad=[0,0,0,0,0];let money=10,ad=0,stars=1,bet=0,spin=null,msg='',msgT=0,over=null,why='',paused=true;
let cust=null,wait=GAP[0],last=performance.now();
const L={x:30,w:250},R={x:620,w:250},C={x:930,w:250},K={x:1240,w:250},M={x:1550,w:250},ROOF=240,WW=1810;let cam=0,clicks=0,bump=0,tm=0,spd=0;const BLK={x:K.x+105,y:250,w:40,h:26};
const plats=[{x:L.x,y:ROOF,w:L.w},{x:R.x,y:ROOF,w:R.w},{x:C.x,y:ROOF,w:C.w},{x:K.x,y:ROOF,w:K.w},{x:M.x,y:ROOF,w:M.w}];
const p={x:330,y:G-28,w:28,h:28,vx:0,vy:0,on:false};
const keys={};
const COLS=['#ffffff','#ff595e','#ff1744','#ff924c','#ff6d00','#ffca3a','#ffeb3b','#c6ff00','#00ff00','#8ac926','#5aa84a','#00e676','#2ec4b6','#18ffff','#1982c4','#2979ff','#6a4c93','#d500f9','#ff7bbf','#f48fb1','#795548','#9aa0a6','#111111'];
const EYES=['Обычные','Большие','Сонные','Злые','Циклоп','Весёлые','Улыбка','Слеза','Удивлённые','Подмигивание','Влюблённые','Крестики','Крутой','Крипер','Пустое','Демон','Прямой рот','22GD','Волшебник','Ведьма','Младенец','Грустный','Слеер','Череп','Богатый','Зельевар','Звёзды в глазах','Перевёрнутое лицо','Подрывник','Угрюмый','Сонный','Добрый'];
let hardBeaten=false;
let beaten=false;
let skin={c:'#ffffff',e:0,m:true,g:null};

const G8=['########','#......#','#.#..#.#','#......#','#.#..#.#','#.####.#','#......#','########'];
function g8(c,px,py,mouth){G8.forEach((row,r)=>{for(let q=0;q<8;q++){if(row[q]!=='#')continue;const m=(r==4&&(q==2||q==5))||(r==5&&q>=2&&q<=5);if(m!==mouth)continue;c.fillRect(px+q*3.5-.25,py+r*3.5-.25,4,4)}})}
function sq(c,px,py,col,es,mo=true,col2=null){
 if(col2){const g=c.createLinearGradient(px,py,px+28,py+28);g.addColorStop(0,col);g.addColorStop(1,col2);c.fillStyle=g}else c.fillStyle=col;c.fillRect(px,py,28,28);c.strokeStyle='#222';c.lineWidth=2;c.strokeRect(px,py,28,28);
 const lum=q=>parseInt(q.slice(1,3),16)*.3+parseInt(q.slice(3,5),16)*.6+parseInt(q.slice(5,7),16)*.1;const h=col2?(lum(col)+lum(col2))/2:lum(col);
 const ec=h<90?'#fff':'#111';
 const X=a=>px+a,Y=a=>py+a,P=Math.PI;
 const arc=(cx,cy,r,a,b)=>{c.beginPath();c.arc(X(cx),Y(cy),r,a,b);c.stroke()};
 const heart=(cx,cy)=>{c.beginPath();c.moveTo(X(cx),Y(cy+4));c.bezierCurveTo(X(cx-7),Y(cy-2),X(cx-2),Y(cy-6),X(cx),Y(cy-1));c.bezierCurveTo(X(cx+2),Y(cy-6),X(cx+7),Y(cy-2),X(cx),Y(cy+4));c.fill()};
 const cr=(a,b,w,h2)=>c.fillRect(X(5+a*3.04),Y(5.8+b*3.04),w*3.04,h2*3.04);
 const ink=()=>{c.fillStyle=ec;c.strokeStyle=ec;c.lineWidth=2};
 const dots=()=>{c.fillRect(X(7),Y(8),4,6);c.fillRect(X(17),Y(8),4,6)};
 ink();
 if(es==0||es==6||es==7||es==16)dots();
 else if(es==1){[9,19].forEach(a=>{c.fillStyle='#fff';c.beginPath();c.arc(X(a),Y(11),4.5,0,7);c.fill();c.strokeStyle='#222';c.lineWidth=1;c.stroke();c.fillStyle='#111';c.beginPath();c.arc(X(a+1),Y(11),2,0,7);c.fill()})}
 else if(es==2){c.fillRect(X(6),Y(11),6,2);c.fillRect(X(16),Y(11),6,2)}
 else if(es==3){c.fillRect(X(7),Y(11),3,3);c.fillRect(X(18),Y(11),3,3);c.beginPath();c.moveTo(X(5),Y(6));c.lineTo(X(12),Y(9));c.moveTo(X(23),Y(6));c.lineTo(X(16),Y(9));c.stroke()}
 else if(es==4){c.fillStyle='#fff';c.beginPath();c.arc(X(14),Y(11),6,0,7);c.fill();c.strokeStyle='#222';c.lineWidth=1;c.stroke();c.fillStyle='#111';c.beginPath();c.arc(X(14),Y(11),3,0,7);c.fill()}
 else if(es==5){arc(9,13,3,P,0);arc(19,13,3,P,0)}
 else if(es==8){c.beginPath();c.arc(X(9),Y(9),3,0,7);c.stroke();c.beginPath();c.arc(X(19),Y(9),3,0,7);c.stroke()}
 else if(es==9){c.fillRect(X(7),Y(8),4,6);c.fillRect(X(16),Y(11),6,2)}
 else if(es==10){c.fillStyle='#e63946';heart(9,10);heart(19,10)}
 else if(es==11){[9,19].forEach(a=>{c.beginPath();c.moveTo(X(a-3),Y(7));c.lineTo(X(a+3),Y(13));c.moveTo(X(a+3),Y(7));c.lineTo(X(a-3),Y(13));c.stroke()})}
 else if(es==12){c.fillStyle='#111';c.fillRect(X(3),Y(7),10,7);c.fillRect(X(15),Y(7),10,7);c.fillRect(X(13),Y(9),2,2)}
 else if(es==15){c.fillStyle='#7a0000';c.beginPath();c.moveTo(X(1),Y(1));c.lineTo(X(8),Y(1));c.lineTo(X(1),Y(9));c.fill();c.beginPath();c.moveTo(X(27),Y(1));c.lineTo(X(20),Y(1));c.lineTo(X(27),Y(9));c.fill();
  c.fillStyle='#ff1f1f';c.beginPath();c.moveTo(X(4),Y(9));c.lineTo(X(12),Y(12));c.lineTo(X(11),Y(15));c.lineTo(X(5),Y(12));c.fill();c.beginPath();c.moveTo(X(24),Y(9));c.lineTo(X(16),Y(12));c.lineTo(X(17),Y(15));c.lineTo(X(23),Y(12));c.fill()}
 else if(es==17){c.fillStyle=ec;g8(c,px,py,false)}
 else if(es==18){c.fillStyle='#3a3aa8';c.beginPath();c.moveTo(X(14),Y(0));c.lineTo(X(5),Y(10));c.lineTo(X(23),Y(10));c.closePath();c.fill();c.fillStyle='#24246e';c.fillRect(X(3),Y(10),22,2.5);c.fillStyle='#ffd400';c.beginPath();c.arc(X(14),Y(6),1.6,0,7);c.fill();c.fillStyle=ec;c.fillRect(X(7),Y(14),4,4);c.fillRect(X(17),Y(14),4,4);c.fillStyle='#f4f4f4';c.beginPath();c.moveTo(X(5),Y(21));c.lineTo(X(23),Y(21));c.lineTo(X(14),Y(28));c.closePath();c.fill();c.strokeStyle='#999';c.lineWidth=1;c.stroke()}
 else if(es==19){c.fillStyle='#1c1c1c';c.beginPath();c.moveTo(X(14),Y(0));c.lineTo(X(6),Y(9));c.lineTo(X(22),Y(9));c.closePath();c.fill();c.fillRect(X(2),Y(9),24,2.5);c.fillStyle='#7a2fd0';c.fillRect(X(8),Y(7),12,2);c.fillStyle='#39ff14';c.fillRect(X(7),Y(13),4,4);c.fillRect(X(17),Y(13),4,4);c.fillStyle='#3a9a1a';c.beginPath();c.arc(X(19),Y(19),1.4,0,7);c.fill()}
 else if(es==20){[9,19].forEach(a=>{c.fillStyle='#fff';c.beginPath();c.arc(X(a),Y(12),4.5,0,7);c.fill();c.strokeStyle='#222';c.lineWidth=1;c.stroke();c.fillStyle='#111';c.beginPath();c.arc(X(a),Y(12.5),2.2,0,7);c.fill()});c.strokeStyle='#8a5a2b';c.lineWidth=2;c.beginPath();c.arc(X(14),Y(3),3,Math.PI*.2,Math.PI*1.7);c.stroke();c.fillStyle='#ff9ab0';c.beginPath();c.arc(X(4.5),Y(19),2.2,0,7);c.fill();c.beginPath();c.arc(X(23.5),Y(19),2.2,0,7);c.fill()}
 else if(es==21){c.fillRect(X(7),Y(10),4,5);c.fillRect(X(17),Y(10),4,5);c.lineWidth=2;c.beginPath();c.moveTo(X(5),Y(8));c.lineTo(X(11),Y(5.5));c.moveTo(X(23),Y(8));c.lineTo(X(17),Y(5.5));c.stroke();c.fillStyle='#4aa3ff';c.fillRect(X(7),Y(16),2,8);c.fillRect(X(19),Y(16),2,8);c.beginPath();c.arc(X(8),Y(25),1.6,0,7);c.fill();c.beginPath();c.arc(X(20),Y(25),1.6,0,7);c.fill()}
 else if(es==27){c.save();c.translate(px+14,py+14);c.rotate(Math.PI);c.translate(-(px+14),-(py+14));dots();c.restore()}
 else if(es==28){dots();c.fillStyle='#111';c.beginPath();c.arc(X(14),Y(5.2),3.2,0,7);c.fill();c.strokeStyle='#8a5a2b';c.lineWidth=1.2;c.beginPath();c.moveTo(X(15.6),Y(2.8));c.lineTo(X(18),Y(1.6));c.stroke();c.fillStyle='#ff8a00';c.beginPath();c.arc(X(18.3),Y(1.5),1.3,0,7);c.fill()}
 else if(es==29){c.fillRect(X(7),Y(11),4,3);c.fillRect(X(17),Y(11),4,3);c.lineWidth=2.4;c.beginPath();c.moveTo(X(5),Y(8));c.lineTo(X(12),Y(10));c.moveTo(X(23),Y(8));c.lineTo(X(16),Y(10));c.stroke()}
 else if(es==30){c.lineWidth=2;c.beginPath();c.arc(X(9),Y(11),3,.2,Math.PI-.2);c.stroke();c.beginPath();c.arc(X(19),Y(11),3,.2,Math.PI-.2);c.stroke();c.lineWidth=1.4;c.beginPath();c.moveTo(X(18),Y(3));c.lineTo(X(22),Y(3));c.lineTo(X(18),Y(6.5));c.lineTo(X(22),Y(6.5));c.stroke()}
 else if(es==31){c.lineWidth=2;c.beginPath();c.arc(X(9),Y(13),3,Math.PI,0);c.stroke();c.beginPath();c.arc(X(19),Y(13),3,Math.PI,0);c.stroke();c.strokeStyle='#ffd23f';c.lineWidth=1.6;c.beginPath();c.ellipse(X(14),Y(2.6),7,2,0,0,7);c.stroke();c.fillStyle='rgba(255,130,160,.6)';c.beginPath();c.arc(X(4.5),Y(17),2.2,0,7);c.fill();c.beginPath();c.arc(X(23.5),Y(17),2.2,0,7);c.fill()}
 else if(es==24){c.save();[9,19].forEach(a=>{c.fillStyle='#ffd23f';c.strokeStyle='#8a6a00';c.lineWidth=1;c.beginPath();c.arc(X(a),Y(11),4.8,0,7);c.fill();c.stroke();c.fillStyle='#7a5a00';c.font='bold 8px system-ui';c.textAlign='center';c.fillText('$',X(a),Y(13.8))});c.restore()}
 else if(es==25){c.save();c.fillStyle='#5a3a1a';c.fillRect(X(2),Y(9.6),24,2.2);[8.5,19.5].forEach(a=>{c.fillStyle='#7a4a1a';c.beginPath();c.arc(X(a),Y(11),5.2,0,7);c.fill();c.fillStyle='#7cfc00';c.beginPath();c.arc(X(a),Y(11),3.8,0,7);c.fill();c.fillStyle='rgba(255,255,255,.75)';c.beginPath();c.arc(X(a-1.2),Y(9.8),1.1,0,7);c.fill()});c.fillStyle='rgba(190,120,255,.9)';[[7,5,1.6],[11,2.8,1.1],[20,4.5,1.4]].forEach(([a,b,r])=>{c.beginPath();c.arc(X(a),Y(b),r,0,7);c.fill()});c.restore()}
 else if(es==26){c.save();c.fillStyle='#ffd23f';c.strokeStyle='#b8860b';c.lineWidth=1;[9,19].forEach(a=>{c.beginPath();for(let k=0;k<10;k++){const ang=-Math.PI/2+k*Math.PI/5,rad=k%2?2.2:5.4,xx=X(a)+Math.cos(ang)*rad,yy=Y(11)+Math.sin(ang)*rad;k?c.lineTo(xx,yy):c.moveTo(xx,yy)}c.closePath();c.fill();c.stroke()});c.fillStyle='#fff';[[4,5],[24,6]].forEach(([a,b])=>{c.fillRect(X(a)-.5,Y(b)-2.5,1,5);c.fillRect(X(a)-2.5,Y(b)-.5,5,1)});c.restore()}
 else if(es==23){c.fillStyle='#f4f4f0';c.strokeStyle='#222';c.lineWidth=1.2;c.beginPath();c.ellipse(X(14),Y(11.5),10,9.5,0,0,7);c.fill();c.stroke();c.fillStyle='#111';c.beginPath();c.ellipse(X(10),Y(11.5),2.8,3.2,0,0,7);c.fill();c.beginPath();c.ellipse(X(18),Y(11.5),2.8,3.2,0,0,7);c.fill();c.beginPath();c.moveTo(X(14),Y(14.5));c.lineTo(X(12.6),Y(17));c.lineTo(X(15.4),Y(17));c.closePath();c.fill()}
 else if(es==22){const w2=col2||'#ffffff';
  if(mo){c.fillStyle=col;c.fillRect(px,py,28,28);c.strokeStyle='#222';c.lineWidth=2;c.strokeRect(px,py,28,28);c.fillStyle=w2;c.fillRect(px+6.6,py+6.5,14.9,14.9)}
  c.strokeStyle='#111';c.lineWidth=1.5;c.strokeRect(px+7.3,py+7.2,13.5,13.5);
  if(mo){c.fillStyle=w2;c.beginPath();if(c.roundRect)c.roundRect(px+10.4,py+10.3,7.4,7.4,1.6);else c.rect(px+10.4,py+10.3,7.4,7.4);c.fill();c.lineWidth=1.5;c.stroke()}}
 else if(es==14){}
 else{cr(0,0,2,2);cr(4,0,2,2)}
 if(es==7){c.fillStyle='#4aa3ff';c.fillRect(X(6),Y(15),2,5)}
 if(!mo)return;
 ink();
 if(es==0)arc(14,18,4,.2,P-.2);
 else if(es==2){c.beginPath();c.ellipse(X(14),Y(21),2.5,3,0,0,7);c.fill()}
 else if(es==3||es==7)arc(14,25,5,P+.25,-.25);
 else if(es==4)arc(14,18,6,.2,P-.2);
 else if(es==5){c.beginPath();c.arc(X(14),Y(17),6,0,P);c.closePath();c.fill()}
 else if(es==6||es==9)arc(14,16,6,.15,P-.15);
 else if(es==1||es==8){c.beginPath();c.ellipse(X(14),Y(20),3,4,0,0,7);c.fill()}
 else if(es==10)arc(14,17,5,.15,P-.15);
 else if(es==11)c.fillRect(X(10),Y(20),8,2);
 else if(es==16)c.fillRect(X(8),Y(20),12,2);
 else if(es==12)arc(14,18,5,.15,P-.15);
 else if(es==15){c.fillStyle='#111';c.beginPath();c.moveTo(X(6),Y(19));c.lineTo(X(22),Y(19));c.lineTo(X(19),Y(25));c.lineTo(X(9),Y(25));c.fill();c.fillStyle='#fff';c.beginPath();c.moveTo(X(8),Y(19));c.lineTo(X(11.5),Y(19));c.lineTo(X(9.5),Y(23.5));c.fill();c.beginPath();c.moveTo(X(16.5),Y(19));c.lineTo(X(20),Y(19));c.lineTo(X(18.5),Y(23.5));c.fill()}
 else if(es==17)g8(c,px,py,true)
 else if(es==18)arc(14,19,3,.15,P-.15)
 else if(es==19){c.beginPath();c.moveTo(X(8),Y(21));c.lineTo(X(12),Y(23.5));c.lineTo(X(16),Y(21));c.lineTo(X(20),Y(23.5));c.stroke()}
 else if(es==20){c.fillStyle='#4fc3f7';c.beginPath();c.ellipse(X(14),Y(21),6,3.2,0,0,7);c.fill();c.fillStyle='#ff9ab0';c.beginPath();c.arc(X(14),Y(21),2.2,0,7);c.fill();c.strokeStyle='#ff7bbf';c.lineWidth=1.6;c.beginPath();c.arc(X(14),Y(25.2),2.2,0,7);c.stroke()}
 else if(es==21)arc(14,26,6,Math.PI+.25,-.25)
 else if(es==27){c.save();c.translate(px+14,py+14);c.rotate(Math.PI);c.translate(-(px+14),-(py+14));arc(14,18,4,.2,P-.2);c.restore()}
 else if(es==28)arc(14,17,5,.15,P-.15)
 else if(es==29)arc(14,25,5,P+.25,-.25)
 else if(es==30){c.beginPath();c.ellipse(X(14),Y(20),2,2.6,0,0,7);c.fill()}
 else if(es==31)arc(14,18,5,.2,P-.2)
 else if(es==24){c.beginPath();c.arc(X(14),Y(17),5.5,.15,Math.PI-.15);c.stroke();c.fillStyle='#ffd23f';c.fillRect(X(16.8),Y(20.6),2.4,2.6)}
 else if(es==25)arc(14,19,4,.2,P-.2)
 else if(es==26){c.beginPath();c.arc(X(14),Y(17),6,0,P);c.closePath();c.fill()}
 else if(es==23){c.fillStyle='#f4f4f0';c.strokeStyle='#222';c.lineWidth=1.2;c.fillRect(X(8),Y(18.5),12,6.5);c.strokeRect(X(8),Y(18.5),12,6.5);c.beginPath();for(let k=1;k<4;k++){c.moveTo(X(8+k*3),Y(18.5));c.lineTo(X(8+k*3),Y(25))}c.moveTo(X(8),Y(21.7));c.lineTo(X(20),Y(21.7));c.stroke()}
 else if(es==22){}
 else if(es==14){}
 else{cr(2,2,2,1);cr(1,3,4,2);cr(1,5,1,1);cr(4,5,1,1)}}
const pv=document.getElementById('pv').getContext('2d');
function menuRender(){if(!canWear(skin.e)||skin.e==6||skin.e==8)skin.e=0;if(!hasAch('tg'))skin.g=null;pv.clearRect(0,0,96,96);pv.save();pv.scale(3,3);sq(pv,2,2,skin.c,skin.e,skin.m,skin.g);pv.restore();
 document.querySelectorAll('#cols .sw').forEach(b=>b.classList.toggle('on',b.dataset.c==skin.c));
 document.querySelectorAll('#cols2 .sw').forEach(b=>b.classList.toggle('on',b.dataset.c==skin.g));
 cols2.style.display=skin.g?'flex':'none';gt.textContent=(hasAch('tg')?'':'🔒 ')+'Градиент: '+(skin.g?'вкл (выбери 2-й цвет)':'выкл');altBtn.textContent=(hasAch('normal')?'':'🔒 ')+'🎭 Альтернативные режимы';gt.classList.toggle('on',!!skin.g);
 ACHS.forEach(a=>{const q=eyes.querySelector('[data-e="'+a.face+'"]');if(q)q.textContent=(hasAch(a.id)?'':'🔒 ')+EYES[a.face]});mt.textContent='Рот: '+(skin.m?'включён':'выключен');mt.classList.toggle('on',skin.m);
 document.querySelectorAll('.eb').forEach(b=>b.classList.toggle('on',+b.dataset.e==skin.e));
 try{localStorage.setItem('shopskin',JSON.stringify(skin))}catch(e){}}
COLS.forEach(c=>{const b=document.createElement('button');b.className='sw';b.dataset.c=c;b.style.background=c;b.onclick=()=>{skin.c=c;menuRender()};cols.appendChild(b)});
COLS.forEach(c=>{const b=document.createElement('button');b.className='sw';b.dataset.c=c;b.style.background=c;b.onclick=()=>{skin.g=c;menuRender()};cols2.appendChild(b)});
function buildPicker(){
[0,14,1,2,3,4,7,9,10,16,5,12,11,17,20,21,24,31,13,18,25,26,27,28,29,30,15,19,22,23].forEach(i=>{
 const n=EYES[i],b=document.createElement('button'),aid=FACE_ACH[i],a=aid&&ACHS.find(q=>q.id==aid);
 b.className='eb';b.dataset.e=i;b.textContent=n;
 if(a){b.style.color=LVC[a.lv];b.style.fontWeight='bold';if(a.lv==1)b.style.textShadow='0 0 2px #000,0 0 2px #000'}
 b.onclick=()=>{if(a&&!hasAch(a.id)){lock.textContent='Чтобы надеть это лицо, получи достижение «'+a.n+'» ('+a.d.toLowerCase()+')';return}lock.textContent='';skin.e=i;menuRender()};
 eyes.appendChild(b)});
}
function menuShow(v){paused=v;menu.style.display=v?'flex':'none';const ip=tm>0.5&&!over;start.textContent=over?'↻ Обычная игра':ip?'▶ Продолжить':'▶ Обычная игра';nm.style.display=ip?'inline-block':'none';if(v)menuRender()}
/* ===== достижения (хранятся в этом браузере) ===== */
const ACHS=[
 {id:'tg',lv:0,n:'Спасибо за поддержку',d:'Нажми кнопку «Телеграм канал»',face:-1,rw:'градиент для скина'},
 {id:'play',lv:0,n:'Теперь я умею играть!',d:'Нажми кнопку «Как играть»',face:5},
 {id:'hardlose',lv:0,n:'Ну я хотя бы попробовал',d:'Проиграй в Hard Mode',face:12},
 {id:'fast',lv:0,n:'Я даже не успел понять, что произошло',d:'Умри за первые 10 секунд игры',face:11},
 {id:'normal',lv:1,n:'Обычный режим',d:'Пройди обычный режим',face:13},
 {id:'hard',lv:2,n:'Hard Mode',d:'Пройди Hard Mode',face:15},
 {id:'double',lv:0,n:'Double Do',d:'Выиграй в казино со ставкой 22 деньги в обычном режиме',face:17},
 {id:'hw',lv:1,n:'Хеллоуинский режим',d:'Пройди «Хеллоуинский режим»',face:18},
 {id:'witch',lv:2,n:'Настоящая ведьма',d:'Пройди «Хеллоуинский режим» с зельем смерти',face:19},
 {id:'kid',lv:0,n:'Детский режим',d:'Пройди «Детский режим»',face:20},
 {id:'kiddie',lv:0,n:'Даже тут ты умер',d:'Умри в «Детском режиме»',face:21},
 {id:'debuff',lv:2,n:'Режим с дебафами с разнообразием',d:'Пройди «Режим с дебафами», выбрав 3 случайных дебафа каждую минуту',face:22},
 {id:'debuff10',lv:2,n:"Режим с дебафами в 10'ти кратном размере",d:'Пройди «Режим с дебафами» с 10 случайными дебафами на всю игру',face:23},
 {id:'rich',lv:0,n:'Миллиардер',d:'Накопи 1 секстилион (число с 21 нулями)',face:24},
 {id:'potions',lv:1,n:'Любитель зелий',d:'Имей 15 действующих эффектов от зелий в обычном режиме',face:25},
 {id:'perfect',lv:1,n:'Перфекционист',d:'Пройди обычный режим так, чтобы после покупки конца игры осталось 0 денег',face:26},
 {id:'spoiledlover',lv:1,n:'Любитель испорченного',d:'Закончи режим «Уничтожь конкурентов» с помощью испорченного товара',face:27},
 {id:'boom',lv:1,n:'БАБАХ!',d:'Закончи режим «Уничтожь конкурентов» с помощью бомбы',face:28},
 {id:'nothere',lv:1,n:'Нет, вам тут не место',d:'Закончи режим «Уничтожь конкурентов» с помощью отказа покупателю',face:29},
 {id:'toolate',lv:1,n:'Ой я не успел',d:'Закончи режим «Уничтожь конкурентов» из-за того, что не успел обслужить покупателя',face:30},
 {id:'altruist',lv:0,n:'Альтруист',d:'Проиграй в режиме «Уничтожь конкурентов», когда рейтинг магазина станет 1000',face:31}];
const LVC=['#3a86ff','#ffd400','#ff2a2a'],LVN=['Лёгкое','Среднее','Сложное'],LVE=['🔵','🟡','🔴'];
const FACE_ACH={};ACHS.forEach(a=>{if(a.face>=0)FACE_ACH[a.face]=a.id});
const ach=new Set();
try{const v=JSON.parse(localStorage.getItem('shopach'));if(Array.isArray(v))v.forEach(id=>{if(ACHS.some(a=>a.id==id))ach.add(id)});
 if(localStorage.getItem('shopwon')=='1')ach.add('normal');if(localStorage.getItem('shophard')=='1')ach.add('hard')}catch(e){}
const rec={n:0,h:0,w:0,k:0,d:0,e:0,r:0};let lastTime=0,newRec=false,lastBest=0;
try{const v=JSON.parse(localStorage.getItem('shoprec'));if(v)['n','h','w','k','d','e','r'].forEach(m=>{if(typeof v[m]=='number'&&v[m]>0)rec[m]=v[m]})}catch(e){}
const fmtT=t=>{const m=Math.floor(t/60),q=(t%60).toFixed(2);return m+':'+(q.length<5?'0':'')+q};
function recUI(){
 nrec.textContent='🏁 Рекорд: '+(rec.n>0?fmtT(rec.n):'—');
 hrec.textContent='🏁 Лучшее время: '+(rec.h>0?fmtT(rec.h):'—');
 wrec.textContent='🏁 Лучшее время: '+(rec.w>0?fmtT(rec.w):'—');
 krec.textContent='🏁 Лучшее время: '+(rec.k>0?fmtT(rec.k):'—');
 drec.textContent='🏁 Лучшее время (3 в минуту): '+(rec.d>0?fmtT(rec.d):'—');
 erec.textContent='🏁 Лучшее время (10 на всю игру): '+(rec.e>0?fmtT(rec.e):'—');
 rrec.textContent='🏁 Лучшее время: '+(rec.r>0?fmtT(rec.r):'—')}
function recWin(){
 const m=hard?'h':hw?'w':kid?'k':deb?(debMode==='all'?'e':'d'):rival?'r':'n';lastTime=tm;newRec=!(rec[m]>0)||tm<rec[m];
 if(newRec){rec[m]=tm;try{localStorage.setItem('shoprec',JSON.stringify(rec))}catch(e){}}
 lastBest=rec[m];recUI()}
const hasAch=id=>ach.has(id);
const canWear=e=>!FACE_ACH[e]||ach.has(FACE_ACH[e]);
function saveAch(){try{localStorage.setItem('shopach',JSON.stringify(Array.from(ach)))}catch(e){}}
function faceCv(e,size){const r=size>40?4:2,c=document.createElement('canvas');c.width=c.height=32*r;c.style.cssText='width:'+size+'px;height:'+size+'px;flex:none';const g=c.getContext('2d');g.scale(r,r);e===22?sq(g,2,2,'#aaaaaa',e,true,'#ffffff'):sq(g,2,2,'#ffffff',e,true,null);return c}
function gradCv(size){const c=document.createElement('canvas');c.width=c.height=size*2;c.style.cssText='width:'+size+'px;height:'+size+'px;flex:none';const g=c.getContext('2d'),gr=g.createLinearGradient(0,0,size*2,size*2);gr.addColorStop(0,'#ff595e');gr.addColorStop(1,'#2979ff');g.fillStyle=gr;g.fillRect(4,4,size*2-8,size*2-8);g.strokeStyle='#222';g.lineWidth=4;g.strokeRect(4,4,size*2-8,size*2-8);return c}
let achQ=[],achBusy=false;
function showAch(){
 if(achBusy||!achQ.length)return;achBusy=true;const a=achQ.shift();
 achBar.textContent='';
 const t=document.createElement('div'),l1=document.createElement('div'),l2=document.createElement('div'),l3=document.createElement('div');
 l1.textContent='🏆 Достижение выполнено!';l1.style.cssText='font-size:12px;opacity:.85';
 l2.textContent=LVE[a.lv]+' '+a.n;l2.style.cssText='font-weight:bold;font-size:15px;color:'+LVC[a.lv];
 l3.textContent='Сложность: '+LVN[a.lv]+(a.face>=0?' · Награда: лицо «'+EYES[a.face]+'»':' · Награда: '+a.rw);l3.style.cssText='font-size:12px';
 t.append(l1,l2,l3);achBar.append(a.face>=0?faceCv(a.face,40):gradCv(40),t);
 achBar.style.display='flex';
 requestAnimationFrame(()=>{achBar.style.opacity='1';achBar.style.transform='translateX(-50%) translateY(0)'});
 setTimeout(()=>{achBar.style.opacity='0';achBar.style.transform='translateX(-50%) translateY(-20px)';setTimeout(()=>{achBar.style.display='none';achBusy=false;showAch()},350)},4500)}
function achRender(){
 achList.textContent='';achCnt.textContent='Выполнено: '+ach.size+' из '+ACHS.length;
 [0,1,2].forEach(lv=>{
  const h=document.createElement('h4'),sh=lv==1?';text-shadow:0 0 2px #000,0 0 2px #000':'';
  h.textContent=LVE[lv]+' '+['Лёгкие','Средние','Сложные'][lv];h.style.cssText='color:'+LVC[lv]+sh;achList.appendChild(h);
  ACHS.filter(a=>a.lv==lv).forEach(a=>{
   const done=ach.has(a.id),row=document.createElement('div'),info=document.createElement('div'),n=document.createElement('div'),d=document.createElement('div'),r=document.createElement('div'),c=a.face>=0?faceCv(a.face,36):gradCv(36);
   row.style.cssText='display:flex;gap:10px;align-items:center;padding:6px 0;border-bottom:1px solid #8884';
   if(!done)c.style.opacity='.45';
   n.textContent=(done?'✅ ':'')+a.n;n.style.cssText='font-weight:bold;font-size:14px;color:'+LVC[lv]+sh;
   d.textContent=a.d;d.style.fontSize='12px';
   r.textContent=a.face>=0?'Награда: глаза «'+EYES[a.face]+'»':'Награда: '+a.rw;r.style.cssText='font-size:12px;font-weight:bold;color:'+LVC[lv]+sh;
   info.append(n,d,r);row.append(c,info);achList.appendChild(row)})})}
function grant(id){
 if(ach.has(id))return;ach.add(id);saveAch();newAch.push(id);
 const a=ACHS.find(q=>q.id==id);if(a){achQ.push(a);showAch()}
 if(achm.style.display==='flex')achRender();
 menuRender()}
function markWin(){recWin();if(rival)return;if(kid){grant('kid');return}if(deb){grant(debMode==='all'?'debuff10':'debuff');return}
 if(hw){grant('hw');if(hwDeath)grant('witch');return}
 try{localStorage.setItem(hard?'shophard':'shopwon','1')}catch(e){}grant(hard?'hard':'normal')}
function applySkin(sk){skin=sk&&COLS.includes(sk.c)&&sk.e>=0&&sk.e<EYES.length&&sk.e!=6&&sk.e!=8?{c:sk.c,e:sk.e,m:sk.m!==false,g:(hasAch('tg')&&COLS.includes(sk.g))?sk.g:null}:{c:'#ffffff',e:0,m:true,g:null};if(!canWear(skin.e))skin.e=0}
{let sv=null;try{sv=JSON.parse(localStorage.getItem('shopskin'))}catch(e){}applySkin(sv)}
achBtn.onclick=()=>{achRender();achm.style.display='flex'};achX.onclick=()=>{achm.style.display='none'};
credBtn.onclick=()=>{cred.style.display='flex'};credX.onclick=()=>{cred.style.display='none'};
buildPicker();recUI();tgLink.addEventListener('click',()=>grant('tg'));
howBtn.onclick=()=>{how.style.display='flex';grant('play')};howX.onclick=()=>{how.style.display='none'};
/* ===== настройки / кнопки движения ===== */
let ctl={s:1,tp:0,js:1,jt:0,l:null,r:null,j:null,jb:false},editing=false,drag=null;
let noPop=false;try{noPop=localStorage.getItem('shoppop')==='off'}catch(e){}
try{const c0=JSON.parse(localStorage.getItem('shopctl'));if(c0){const cl=(v,a,b,d)=>typeof v=='number'?Math.max(a,Math.min(b,v)):d;
 const ok=p=>p&&typeof p.x=='number'&&typeof p.y=='number'?{x:Math.max(0,Math.min(1,p.x)),y:Math.max(0,Math.min(1,p.y))}:null;
 ctl.s=cl(c0.s,.5,3,1);ctl.tp=cl(c0.tp,0,100,0);ctl.js=cl(c0.js,.5,3,1);ctl.jt=cl(c0.jt,0,100,0);ctl.l=ok(c0.l);ctl.r=ok(c0.r);ctl.j=ok(c0.j);ctl.jb=!!c0.jb}}catch(e){}
function saveCtl(){try{localStorage.setItem('shopctl',JSON.stringify(ctl))}catch(e){}}
function applyCtl(){
 const W=wrap.clientWidth||900,H=wrap.clientHeight||420,w=43*ctl.s,h=32*ctl.s,q=64*ctl.js;
 const op=t=>editing?Math.max(.35,1-t/100):1-t/100;
 const place=(el,p,dx,ww,hh,fs,rad,o)=>{
  el.style.width=ww+'px';el.style.height=hh+'px';el.style.fontSize=fs+'px';el.style.borderRadius=rad+'px';el.style.opacity=o;
  if(p){el.style.left=Math.max(0,Math.min(W-ww,p.x*W))+'px';el.style.bottom=Math.max(0,Math.min(H-hh,p.y*H))+'px'}
  else{el.style.left=Math.max(0,dx)+'px';el.style.bottom='14px'}};
 place(tl,ctl.l,10,w,h,14*ctl.s,6*ctl.s,op(ctl.tp));place(tr,ctl.r,10+w+8,w,h,14*ctl.s,6*ctl.s,op(ctl.tp));
 place(tj,ctl.j,W-q-10,q,q,22*ctl.js,6*ctl.js,op(ctl.jt));
 tj.style.display=ctl.jb?'flex':'none'}
function ctlUI(){
 ctlR.value=ctl.s;ctlLab.textContent='×'+ctl.s.toFixed(1);ctlT.value=ctl.tp;ctlTL.textContent=ctl.tp+'%';
 ctlRJ.value=ctl.js;ctlLabJ.textContent='×'+ctl.js.toFixed(1);ctlTJ.value=ctl.jt;ctlTLJ.textContent=ctl.jt+'%'}
function finishCtl(){editing=false;ctlBar.style.display='none';[tl,tr,tj].forEach(q=>q.classList.remove('edit'));applyCtl();menuShow(true)}
ctlBtn.onclick=()=>{setm.style.display='none';editing=true;menu.style.display='none';ctlBar.style.display='flex';ctlUI();[tl,tr,tj].forEach(q=>q.classList.add('edit'));jLab();applyCtl()};
ctlR.oninput=()=>{ctl.s=+ctlR.value;ctlUI();applyCtl();saveCtl()};
ctlT.oninput=()=>{ctl.tp=+ctlT.value;ctlUI();applyCtl();saveCtl()};
ctlRJ.oninput=()=>{ctl.js=+ctlRJ.value;ctlUI();applyCtl();saveCtl()};
ctlTJ.oninput=()=>{ctl.jt=+ctlTJ.value;ctlUI();applyCtl();saveCtl()};
ctlReset.onclick=()=>{ctl={s:1,tp:0,js:1,jt:0,l:null,r:null,j:null,jb:false};ctlUI();jLab();applyCtl();saveCtl()};
ctlOk.onclick=finishCtl;
const jLab=()=>{ctlJ.textContent='Прыжок: '+(ctl.jb?'отдельная кнопка':'весь экран')};
ctlJ.onclick=()=>{ctl.jb=!ctl.jb;jLab();applyCtl();saveCtl()};
[[tl,'l'],[tr,'r'],[tj,'j']].forEach(([el,key])=>el.addEventListener('pointerdown',e=>{
 if(!editing)return;e.preventDefault();e.stopPropagation();
 const r=el.getBoundingClientRect();drag={el,key,ox:e.clientX-r.left,oy:r.bottom-e.clientY};try{el.setPointerCapture(e.pointerId)}catch(_){}}));
addEventListener('pointermove',e=>{
 if(!drag)return;
 const R=wrap.getBoundingClientRect(),W=R.width,H=R.height,w=drag.el.offsetWidth,h=drag.el.offsetHeight;
 const left=Math.max(0,Math.min(W-w,e.clientX-drag.ox-R.left)),bottom=Math.max(0,Math.min(H-h,R.bottom-e.clientY-drag.oy));
 ctl[drag.key]={x:left/W,y:bottom/H};applyCtl()});
addEventListener('pointerup',()=>{if(drag){drag=null;saveCtl()}});
addEventListener('resize',applyCtl);
/* --- окно «Настройки» --- */
function setLabels(){
 popBtn.textContent=noPop?'🪟 Показывать всплывающие окна у магазинов':'🪟 Убрать всплывающие окна у магазинов';
 setG.textContent=gfxOld?'🎨 Включить новую графику':'🎨 Включить старую графику'}
setBtn.onclick=()=>{setLabels();setm.style.display='flex'};
setX.onclick=()=>{setm.style.display='none'};
popBtn.onclick=()=>{noPop=!noPop;try{localStorage.setItem('shoppop',noPop?'off':'on')}catch(e){}setLabels()};
setG.onclick=()=>{gfxOld=!gfxOld;try{localStorage.setItem('shopgfx',gfxOld?'old':'new')}catch(e){}setLabels()};
applyCtl();
gt.onclick=()=>{if(!hasAch('tg')){lock.textContent='Чтобы открыть градиент, получи достижение «Спасибо за поддержку» (нажми «✈ Телеграм канал»)';return}lock.textContent='';skin.g=skin.g?null:(skin.c=='#2979ff'?'#ff1744':'#2979ff');menuRender()};
mt.onclick=()=>{skin.m=!skin.m;menuRender()};
start.onclick=()=>{if(over)reset(false);menuShow(false)};
hm.onclick=()=>{reset(true);altm.style.display='none';menuShow(false)};hwBtn.onclick=()=>{hwCh.style.display='flex'};hwNo.onclick=()=>{hwChoice=false;reset('w');hwCh.style.display='none';altm.style.display='none';menuShow(false)};hwYes.onclick=()=>{hwChoice=true;reset('w');hwCh.style.display='none';altm.style.display='none';menuShow(false)};hwChX.onclick=()=>{hwCh.style.display='none'};hwInfoBtn.onclick=()=>{hwInfo.style.display=hwInfo.style.display=='none'?'block':'none'};altBtn.onclick=()=>{if(!hasAch('normal')){lock.textContent='Альтернативные режимы откроются после прохождения обычного режима';return}lock.textContent='';altm.style.display='flex'};altX.onclick=()=>{altm.style.display='none'};kidBtn.onclick=()=>{reset('k');altm.style.display='none';menuShow(false)};kidInfoBtn.onclick=()=>{kidInfo.style.display=kidInfo.style.display=='none'?'block':'none'};nm.onclick=()=>{reset(false);menuShow(false)};hi.onclick=()=>{hinfo.style.display=hinfo.style.display=='none'?'block':'none'};bm.onclick=()=>menuShow(true);
document.addEventListener('click',e=>{if(e.target.blur&&e.target.tagName!=='INPUT')e.target.blur()});
function csq(c,px,py,col,m){
 c.fillStyle=col;c.fillRect(px,py,28,28);c.strokeStyle='#222';c.lineWidth=2;c.strokeRect(px,py,28,28);
 c.fillStyle='#111';c.fillRect(px+7,py+8,4,5);c.fillRect(px+17,py+8,4,5);c.lineWidth=2;c.beginPath();
 if(m==2)c.arc(px+14,py+16,5,.1,Math.PI-.1);
 else if(m==1){c.moveTo(px+9,py+21);c.lineTo(px+19,py+21)}
 else{c.arc(px+14,py+25,5,Math.PI+.2,-.2);c.moveTo(px+5,py+4);c.lineTo(px+12,py+7);c.moveTo(px+23,py+4);c.lineTo(px+16,py+7)}
 c.stroke()}
const FXN={grow:'⬆ Увеличение',shrink:'⬇ Уменьшение',speed:'👟 Скорость +1',profit:'💰 Прибыль +5%',anti:'📉 Анти-прибыль −5%',luck:'🍀 Удача (шансы ×2)',bad:'🌑 Неудача (шансы ×2)'};
{const ft=x.fillText.bind(x);x.fillText=function(t,...a){if(hw){t=String(t).replace(/💰 ?\$/g,'🍬 ').replace(/\$(?=\d)/g,'🍬').replace(/денег/g,'конфет').replace(/деньги/g,'конфеты')}return ft(t,...a)}}
let fx=[],sc=1,brewing=null,pmenu=false;
let hw=false,hwT=60,hwLvl=0,hwDeath=false,hwChoice=false,rvLast='';
let rival=false,rating=500,spoilOn=false,rivalT=0,sales=0,newAch=[];const HWI=[60,30,15,10,5,1],HWC=[50,100,200,400,800];
const cnt=k=>fx.filter(e=>e.k==k).length;
const net=()=>debActive(13)?3:debActive(14)?-3:Math.max(-3,Math.min(3,cnt('grow')-cnt('shrink')));
const spLvl=()=>Math.min(SPD.length,spd+cnt('speed'));
const tipBonus=()=>5*(cnt('profit')-cnt('anti'));
function applySize(){const ns=Math.pow(2,net());if(ns!=sc){const cx=p.x+p.w/2,b=p.y+p.h;sc=ns;p.w=p.h=28*ns;p.x=Math.max(0,Math.min(WW-p.w,cx-p.w/2));p.y=b-p.h}}
function brew(){if(over||paused||brewing)return;if(hw){say('В Хеллоуинском режиме случайное зелье купить нельзя');return}const cost=kid?1:hard?1000:100;if(money<cost){say('Нужно $'+cost+' на зелье!');return}money-=cost;if(kid){finishBrew();return}brewing={t:1.5};say('Варим зелье… −$'+cost)}
const canApply=r=>r==0?net()<3:r==1?net()>-3:r==2?spLvl()<SPD.length:true;
function choose(k){
 if(!canApply(k)){say('Этот эффект сейчас нельзя применить (лимит)');return}
 const cost=kid?1:hw?100:1000;
 if(money<cost){say('Нужно $'+cost+'!');pmenu=false;return}
 money-=cost;pmenu=false;finishBrew(k)}
function respawn(){
 if(over!=='lose')return;
 if(!kid){say('В этом режиме возродиться нельзя');return}
 const rc=kid?0:500;if(money<rc){say('Не хватает денег на возрождение — нужно $500');return}
 money-=rc;
 over=null;why='';if(stars<=0)stars=1;if(money<0)money=0;rej=0;cust=null;wait=GAP[ad];spin=null;say('✨ Ты возродился!')}
function finishBrew(rr){brewing=null;const r=rr===undefined?roll():rr,add=k=>fx.push({k,t:60});
 potCnt++;if(deb){if(debActive(10)){debFail(10);return}if(debActive(15)&&[0,2,3,5].includes(r)){debFail(15);return}if(debActive(13)&&r==1){debFail(13);return}if(debActive(14)&&r==0){debFail(14);return}}
 if(r==0){if(net()>=3)say('Размер уже ×8 — зелье пропало');else{add('grow');say('Зелье увеличения! ×2 на 1 минуту')}}
 else if(r==1){if(net()<=-3)say('Размер уже ×1/8 — зелье пропало');else{add('shrink');say('Зелье уменьшения! ÷2 на 1 минуту')}}
 else if(r==2){if(spLvl()>=SPD.length)say('Скорость уже максимальная — зелье пропало');else{add('speed');say('Зелье скорости! +1 уровень на 1 минуту')}}
 else if(r==3){add('profit');say('Зелье прибыли! Чаевые +5% на 1 минуту')}
 else if(r==4){add('anti');say('Зелье анти-прибыли! Чаевые −5% на 1 минуту')}
 else if(r==5){add('luck');say('Зелье удачи! Шансы на хорошее ×2 на 1 минуту')}
 else if(r==6){add('bad');say('Зелье неудачи! Шансы на плохое ×2 на 1 минуту')}
 else{lose('Зелье смерти!');say('☠ Зелье смерти!')}}
const cn=()=>kid?1:hard?50:10;
const mul=()=>hard?2:1,adP=()=>kid?1:ADS[ad],spP=()=>kid?1:SPD[spd]*mul(),goal=()=>GOAL;
const roll=()=>{const w=hard?[1,2,1,1,2,1,1,3]:[1,1,1,1,1,1,1,1],good=[0,2,3,5],gm=cnt('luck')?2:1,bm=cnt('bad')?2:1,ww=w.map((v,i)=>v*(good.includes(i)?gm:bm));if(hw&&!hwDeath)ww[7]=0;if(kid)[1,4,6,7].forEach(i=>{ww[i]=0});if(debActive(15))good.forEach(i=>{ww[i]=0});let r=Math.random()*ww.reduce((p,q)=>p+q,0),i=0;while(r>=ww[i]){r-=ww[i];i++}return i};
const fmt=t=>Math.floor(t/60)+':'+String(Math.floor(t%60)).padStart(2,'0');
const starTxt=()=>'★'.repeat(Math.floor(stars))+(stars%1?'½':'')+' '+stars.toFixed(1);
const SP={2:30,3:50,4:100,5:300,6:500,7:1000,8:1500,9:3000,10:5000},TIPS={1:[-5,0],2:[0,0],3:[0,3],4:[0,5],5:[3,5],6:[3,10],7:[5,10],8:[5,25],9:[10,25],10:[25,100]};
let rej=0;
const starPrice=()=>stars>=5?0:(kid?1:SP[Math.round(stars*2)+1]);
const tipRange=()=>TIPS[Math.round(stars*2)]||[0,0];
const tipPct=()=>{if(hard||rival||debActive(22))return 0;const t=tipRange();return t[0]+Math.random()*(t[1]-t[0])+tipBonus()};
function reset(h){hard=(h===true);hw=(h==='w');kid=(h==='k');deb=(h==='d');rival=(h==='r');sales=0;newAch=[];rating=500;rivalT=0;spoilOn=false;bad.fill(0);bf.style.display=rival?'inline-block':'none';debList=[];debReveal=null;debT=0;hwT=60;hwLvl=0;hwDeath=(h==='w')&&hwChoice;PAT=hard?1:kid?1e9:20;pmenu=false;fx=[];sc=1;p.w=p.h=28;brewing=null;spd=0;tm=0;rej=0;clicks=0;inv.fill(0);money=10;ad=0;stars=1;bet=0;spin=null;cust=null;wait=GAP[0];over=null;why='';msgT=0;p.x=330;p.y=G-28;p.vy=0;if(kid){stars=5;spd=SPD.length;ad=ADS.length;wait=GAP[ad]}if(deb)debPick()}
function lose(r){if(!over){over='lose';why=r;if(kid)grant('kiddie');if(hard)grant('hardlose');if(tm<=10)grant('fast')}}
function leave(t){if(rival){cust=null;wait=GAP[ad];say(t+' · рейтинг −25');ratingAdd(-25,'late');return}if(hard){cust=null;stars=0;say(t+' ★ все звёзды потеряны');lose('Клиент не обслужен — в Hard Mode это конец игры');return}
 cust=null;wait=GAP[ad];stars=Math.max(0,stars-0.5);say(t+' ★ −0.5');if(stars<=0)lose('Звёзды магазина закончились')}
function say(t){msg=t;msgT=110}
function inside(s){return p.x+p.w/2>s.x&&p.x+p.w/2<s.x+s.w&&p.y+p.h>ROOF+4}
const has=()=>inv.some((n,i)=>n>0||(rival&&bad[i]>0));
function pick(){const o=inv.map((n,i)=>(n>0||(rival&&bad[i]>0))?i:-1).filter(i=>i>=0);return o[Math.floor(Math.random()*o.length)]}
function sell(){
 if(!inside(L)||over||paused)return;
 if(!cust){say('Покупателя пока нет');return}
 const i=cust.i;
 if(rival){sellRival(false);return}
 if(inv[i]>0){sales++;inv[i]--;const tip=Math.round(items[i].sell*tipPct()/100);money+=items[i].sell+tip;say('Продано: '+items[i].n+' +$'+items[i].sell+(tip?(tip>0?' + чаевые $':' − штраф $')+Math.abs(tip):''));cust=null;wait=GAP[ad];if(deb&&debActive(18)){perfSales++;if(perfSales>=10)debFail(18)}}
 else say('У тебя нет: '+items[i].n)}
const RVAL=[1,5,10,25,50];
function ratingAdd(d,cause){
 if(over)return;
 rating=Math.max(0,Math.min(1000,rating+d));
 if(rating<=0)rivalWin(cause);
 else if(rating>=1000){lose('Рейтинг магазина достиг 1000');grant('altruist')}}
function rivalWin(cause){
 if(over)return;
 over='win';markWin();
 const m={spoiled:'spoiledlover',bomb:'boom',refuse:'nothere',late:'toolate'}[cause];if(m)grant(m)}
function spoil(ix){
 const it=items[ix],cost=Math.ceil(it.buy*.25);
 if(inv[ix]<=0){say('Нечего портить: сначала купи '+it.n);return}
 if(money<cost){say('Не хватает денег! Нужно $'+cost);return}
 money-=cost;inv[ix]--;bad[ix]++;say('🤢 Испорчено: '+it.n+' −$'+cost)}
function sellRival(forceBad){
 if(!inside(L)||over||paused)return;
 if(!cust){say('Покупателя пока нет');return}
 const i=cust.i,useBad=forceBad||(inv[i]<=0&&bad[i]>0);
 if(useBad){
  if(bad[i]<=0){say('Нет испорченного: '+items[i].n);return}
  sales++;bad[i]--;const g=Math.round(items[i].sell*.5);money+=g;cust=null;wait=GAP[ad];
  say('🤢 Продан испорченный: '+items[i].n+' +$'+g+' · рейтинг −'+RVAL[i]);ratingAdd(-RVAL[i],'spoiled')}
 else{
  if(inv[i]<=0){say('У тебя нет: '+items[i].n);return}
  sales++;inv[i]--;money+=items[i].sell;cust=null;wait=GAP[ad];
  say('Продано: '+items[i].n+' +$'+items[i].sell)}}
function sellBad(){if(rival)sellRival(true)}
function drawRating(){
 x.textAlign='center';x.font='bold 12px system-ui';x.fillStyle='#ffd23f';
 x.fillText('🏪 Рейтинг: '+rating+' / 1000  ·  🤢 🍎'+bad[0]+' 🥕'+bad[1]+' 🥛'+bad[2]+' 🧀'+bad[3]+' 🗡'+bad[4],450,62);
 x.fillStyle='rgba(255,255,255,.2)';x.fillRect(262,66,376,5);
 x.fillStyle=rating>700?'#e63946':rating>300?'#ffb703':'#52b788';x.fillRect(262,66,376*rating/1000,5)}
function restart(){if(over)reset(hard?true:hw?'w':kid?'k':deb?'d':rival?'r':false)}
function reject(){
 if(pmenu&&inside(M)){pmenu=false;say('Меню закрыто');return}
 if(!inside(L)||over||paused)return;
 if(!cust){say('Отказывать некому');return}
 perfSales=0;
 if(rival){cust=null;wait=GAP[ad];say('Ты отказал покупателю · рейтинг −1');ratingAdd(-1,'refuse');return}
 if(kid){cust=null;wait=GAP[ad];say('Ты отказал покупателю (звёзды не теряются)');return}
 if(hard){cust=null;stars=0;say('Отказ! ★ все звёзды потеряны');lose('Отказ клиенту — в Hard Mode это конец игры');return}
 rej++;cust=null;wait=GAP[ad];if(rej>=3){rej=0;stars=Math.max(0,stars-0.5);say('Третий отказ подряд! ★ −0.5');if(stars<=0)lose('Звёзды магазина закончились')}else say('Ты отказал покупателю ('+rej+'/3)')}
function resolveSpin(){window.lastR=spin.r;const d=spin.r==2?spin.b:spin.r==-2?-spin.b:0;money+=d;if(spin.r===2&&spin.b===22&&!hard&&!hw&&!kid&&!deb)grant('double');if(spin.r===-2&&debActive(8))debFail(8);say(spin.r==0?'Выпало X0 — ничего':'Выпало X'+spin.r+'! '+(d>0?'+':'−')+'$'+Math.abs(d));spin=null}
function doSpin(){
 if(over||paused||spin)return;
 if(bet<=0){say('Сначала сделай ставку (1)');return}
 if(bet>money){bet=Math.max(0,Math.floor(money));say('Ставка уменьшена до баланса');return}
 {const wW=cnt('luck')?2:1,wL=(hard?2:1)*(cnt('bad')?2:1);spin={t:1.6,r:debActive(15)?-2:(kid||Math.random()<wW/(wW+wL))?2:-2,b:bet};casCnt++}if(kid)resolveSpin()}
function use(){if(inside(C))doSpin();else if(inside(M))brew();else sell()}
function addBet(n){if(bet+n>money)say('Ставка не может быть больше баланса ($'+Math.floor(money)+')');else bet+=n}
function act(k){
 if(over||paused)return;
 if(rival&&spoilOn&&inside(R)&&k<5){spoil(k);return}
 if(inside(M)){
  if(pmenu){if(k<(hw?7:8))choose(k);else say('Выбери эффект из списка (Q — отмена)')}
  else if(hw){
   if(k==0){if(money<100)say('Нужно $100!');else pmenu=true}
   else if(k==1){if(hwLvl>=5)say('Скорость эффектов уже максимальная');else if(money>=HWC[hwLvl]){money-=HWC[hwLvl];hwLvl++;hwT=Math.min(hwT,HWI[hwLvl]);say('Новый эффект каждые '+HWI[hwLvl]+' с!')}else say('Нужно $'+HWC[hwLvl])}
   
   else say('Дом магии: 1 — выбрать зелье ($100), 2 — ускорить эффекты')}
  else if(k==0){if(hard)say('Hard Mode: выбрать зелье нельзя');else if(money<(kid?1:1000))say('Нужно $'+(kid?1:1000)+'!');else pmenu=true}
  else say('В доме магии: E — случайное зелье ($100), 1 — выбрать эффект ($1000)');
  return}
 if(inside(C)){if(spin)return;if(k==0)addBet(10);else if(k==1)addBet(100);else if(k==2)addBet(1000);else if(k==3)bet=Math.max(0,Math.floor(money));else if(k==4)bet=0;else if(k==5)addBet(1);else say('В казино: 1/2/3 — +10/+100/+1000, 6 — +1, 4 — МАКС, 5 — сброс, E — крутить');return}
 if(k<5){const ix=k,it=items[ix];if(hard&&!inside(R)){say('Hard Mode: доставки нет — покупай в правом магазине');return}const pr=kid?1:hard?it.out:(inside(R)?it.buy:it.out);
  if(money>=pr){money-=pr;inv[ix]++;say('Куплено: '+it.n+' −$'+pr);debBuy(ix,it)}else say('Не хватает денег! Нужно $'+pr)}
 else if(!inside(R))say('Это можно купить только в правом магазине');
 else if(rival&&k==7){spoilOn=!spoilOn;say(spoilOn?'🤢 Режим порчи ВКЛ: жми 1–5, чтобы испортить товар':'Режим порчи выключен')}
 else if(rival&&k==8){if(money<20000)say('Бомба стоит $20000 — не хватает денег!');else{money-=20000;say('💣 БАБАХ!');rivalWin('bomb')}}
 else if(k==5){
  if(ad>=ADS.length)say('Реклама уже на максимуме!');
  else if(money>=adP()){money-=adP();ad++;wait=GAP[ad];say('Реклама! Покупатель каждые '+GAP[ad]+' с')}
  else say('Не хватает денег! Нужно $'+adP())}
 else if(k==8){if(stars>=5&&money>=goal()){money-=goal();over='win';markWin();if(!hard&&!hw&&!kid&&!deb&&money===0)grant('perfect')}else say('Чтобы закончить игру нужно ★5 и $'+goal()+' (сейчас ★'+stars.toFixed(1)+', $'+Math.floor(money)+')')}
 else if(k==6){if(spd>=SPD.length)say('Скорость уже максимальная!');else if(money>=spP()){money-=spP();spd++;say('Скорость ур.'+spd+'!')}else say('Не хватает денег! Нужно $'+spP())}
 else if(k==7){const pr=starPrice();if(stars>=5)say('Звёзд уже максимум!');else if(money>=pr){money-=pr;stars+=0.5;say('+0.5 звезды! Теперь '+stars.toFixed(1))}else say('Не хватает денег! Нужно $'+pr)}}
addEventListener('keydown',e=>{if(how.style.display==='flex'||achm.style.display==='flex'||cred.style.display==='flex'||altm.style.display==='flex'||setm.style.display==='flex'||hwCh.style.display==='flex'||debCh.style.display==='flex'||rivalIntro.style.display==='flex'){if(e.code=='Escape'){how.style.display='none';achm.style.display='none';cred.style.display='none';altm.style.display='none';setm.style.display='none';hwCh.style.display='none';debCh.style.display='none';rivalIntro.style.display='none'}return}
 if(editing){if(e.code=='Escape')finishCtl();return}
 keys[e.code]=true;
 if(['ArrowUp','ArrowLeft','ArrowRight','Space'].includes(e.code)&&!paused)e.preventDefault();
 if(e.repeat)return;
 if(e.code=='KeyE'||e.code=='Enter')use();
 if(e.code=='KeyR')restart();
 if(e.code=='KeyV')respawn();
 if(e.code=='KeyF')sellBad();
 if(e.code=='KeyQ'||e.code=='KeyX')reject();
 if(e.code=='Escape')menuShow(!paused);
 const m=e.code.match(/^(Digit|Numpad)([1-9])$/);if(m)act(+m[2]-1)});
addEventListener('keyup',e=>keys[e.code]=false);
const hold=(id,k)=>{const b=document.getElementById(id);
 b.addEventListener('pointerdown',e=>{e.preventDefault();keys[k]=true});
 ['pointerup','pointerleave','pointercancel'].forEach(ev=>b.addEventListener(ev,()=>keys[k]=false))};
const arrP=new Map();
function arrSync(){let l=false,r=false;arrP.forEach(d=>{if(d==='l')l=true;if(d==='r')r=true});keys.TL=l;keys.TR=r}
function arrDir(e){
 const a=tl.getBoundingClientRect(),b=tr.getBoundingClientRect(),m=14,x0=e.clientX,y0=e.clientY;
 const near=r=>x0>=r.left-m&&x0<=r.right+m&&y0>=r.top-m&&y0<=r.bottom+m;
 const dl=near(a),dr=near(b);
 if(dl&&dr)return Math.abs(x0-(a.left+a.right)/2)<=Math.abs(x0-(b.left+b.right)/2)?'l':'r';
 return dl?'l':dr?'r':null}
[tl,tr].forEach(el=>el.addEventListener('pointerdown',e=>{if(editing)return;e.preventDefault();try{el.releasePointerCapture(e.pointerId)}catch(_){}arrP.set(e.pointerId,el===tl?'l':'r');arrSync()}));
addEventListener('pointermove',e=>{if(!arrP.has(e.pointerId))return;arrP.set(e.pointerId,arrDir(e));arrSync()});
['pointerup','pointercancel'].forEach(ev=>addEventListener(ev,e=>{if(arrP.delete(e.pointerId))arrSync()}));
hold('tj','TJ');
cv.addEventListener('pointerdown',e=>{if(ctl.jb)return;e.preventDefault();keys.TJ=true});
['pointerup','pointercancel','pointerleave'].forEach(ev=>cv.addEventListener(ev,()=>{keys.TJ=false}));
addEventListener('pointerup',()=>{keys.TJ=false});wrap.addEventListener('contextmenu',e=>e.preventDefault());
document.querySelectorAll('[data-k]').forEach(b=>b.addEventListener('click',()=>act(+b.dataset.k-1)));
rv.onclick=respawn;be.onclick=use;bx.onclick=reject;

function update(dt){
 applySize();
 const l=keys.ArrowLeft||keys.KeyA||keys.TL,r=keys.ArrowRight||keys.KeyD||keys.TR,j=keys.ArrowUp||keys.KeyW||keys.Space||keys.TJ;
 const sp=4+spLvl()*0.75;p.vx=(r?1:0)*sp-(l?1:0)*sp;
 if(j&&p.on){p.vy=-9.5*Math.pow(sc,1/3);p.on=false;if(debActive(16))debFail(16)}
 p.vy+=0.55;const py=p.y+p.h;
 p.x=Math.max(0,Math.min(WW-p.w,p.x+p.vx));p.y+=p.vy;p.on=false;
 if(p.vy<0&&!over&&p.x+p.w>BLK.x&&p.x<BLK.x+BLK.w&&py-p.h>=BLK.y+BLK.h&&p.y<BLK.y+BLK.h){p.y=BLK.y+BLK.h;p.vy=1;clicks++;bump=8;if(debActive(9))debFail(9);if(clicks%cn()==0){money+=1;say('Кликер: '+cn()+' ударов = +$1')}}
 for(const s of plats)
  if(p.vy>=0&&py<=s.y+1&&p.y+p.h>=s.y&&p.x+p.w>s.x&&p.x<s.x+s.w){p.y=s.y-p.h;p.vy=0;p.on=true}
 if(p.y+p.h>=G){p.y=G-p.h;p.vy=0;p.on=true}
 cam=Math.max(0,Math.min(WW-900,p.x+p.w/2-450));
 if(msgT>0)msgT--;
 if(bump>0)bump--;
 if(!over)tm+=dt;
 if(hard&&!over&&tm>=1500)lose('Время Hard Mode вышло!');
 if(over)return;
 if(pmenu&&!inside(M))pmenu=false;
 for(const e of fx)e.t-=dt;fx=fx.filter(e=>e.t>0);
 if(brewing){brewing.t-=dt;if(brewing.t<=0)finishBrew()}
 if(hw){hwT-=dt;if(hwT<=0){hwT=HWI[hwLvl];finishBrew(roll())}}
 if(deb)debTick(dt);
 if(rival&&!over){rivalT+=dt;while(rivalT>=1&&!over){rivalT-=1;ratingAdd(1,'tick')}}
 if(money>=1e21)grant('rich');
 if(!hard&&!hw&&!kid&&!deb&&fx.length>=15)grant('potions');
 if(spin){spin.t-=dt;if(spin.t<=0)resolveSpin()}
 if(money<0)lose('Баланс ушёл в минус');
 if(over)return;
 if(cust){cust.t-=dt;if(cust.t<=0)leave('Покупатель ушёл.')}
 else if(has()){wait-=dt;if(wait<=0)cust={i:pick(),t:PAT,col:'hsl('+Math.floor(Math.random()*360)+',60%,60%)'}}
}
function itemOld(i,px,py,s){x.fillStyle=items[i].c;x.strokeStyle='#0004';x.lineWidth=1;
 if(i==0){x.beginPath();x.arc(px,py,s,0,7);x.fill();x.stroke()}
 else if(i==1){x.beginPath();x.moveTo(px-s,py-s);x.lineTo(px+s,py-s);x.lineTo(px,py+s*1.4);x.fill();x.stroke()}
 else if(i==2){x.fillRect(px-s*.7,py-s,s*1.4,s*2);x.strokeRect(px-s*.7,py-s,s*1.4,s*2)}
 else if(i==4){x.fillRect(px-1.5,py-s*1.6,3,s*2.3);x.fillStyle='#8a5a2b';x.fillRect(px-s*.8,py+s*.7,s*1.6,2.5);x.fillRect(px-1.2,py+s*.7,2.4,s*.9)}
 else{x.beginPath();x.moveTo(px-s,py+s);x.lineTo(px+s,py+s);x.lineTo(px+s,py-s*.2);x.lineTo(px-s,py-s);x.closePath();x.fill();x.stroke()}}
function shop(s,name,col,cas){
 x.fillStyle=col;x.fillRect(s.x,ROOF,s.w,G-ROOF);
 if(cas!=='clk'&&cas!=='magic'){x.fillStyle='#0002';x.fillRect(s.x,ROOF+70,s.w,50);x.fillStyle='#8a5a2b';x.fillRect(s.x+s.w/2-40,G-40,s.w/2+40,40)}
 x.fillStyle='#6b3f1d';x.fillRect(s.x-8,ROOF-10,s.w+16,12);
 x.fillStyle='#fff';x.fillRect(s.x+50,ROOF-34,s.w-100,24);x.strokeStyle='#222';x.lineWidth=2;x.strokeRect(s.x+50,ROOF-34,s.w-100,24);
 x.fillStyle='#222';x.font='bold 15px system-ui';x.textAlign='center';x.fillText(name,s.x+s.w/2,ROOF-17);
 if(cas==='clk'||cas==='magic'){}
 else if(cas){for(let i=0;i<3;i++){x.fillStyle='#222';x.fillRect(s.x+14+i*30,ROOF+50,22,66);x.fillStyle='#ffd23f';x.font='bold 14px system-ui';x.textAlign='center';x.fillText('7',s.x+25+i*30,ROOF+78)}}
 else for(let i=0;i<5;i++)item(i,s.x+22+i*28,ROOF+60,7)}
function box(px,py,w,h){x.fillStyle='#000c';x.fillRect(px,py,w,h);x.fillStyle='#fff';x.textAlign='left'}
function buyPanel(){
 const w=240,h=127,px=R.x+5,py=79;box(px,py,w,h);
 x.font='bold 13px system-ui';x.fillText(rival&&spoilOn?'🤢 ПОРЧА (25% цены): жми 1–5':'Купить  (клиент раз в '+GAP[ad]+' с)',px+10,py+15);x.font='11.5px system-ui';
 const em=['🍎','🥕','🥛','🧀','🗡'],pc=i=>'$'+(kid?1:hard?items[i].out:items[i].buy)+'  (есть '+inv[i]+(rival?', 🤢'+bad[i]:'')+')';
 const rows=items.slice(0,5).map((it,i)=>[(i+1)+'. '+em[i]+' '+it.n,pc(i)]);
 rows.push(['6. 📢 Реклама'+(ad<ADS.length?' ур.'+(ad+1):''),ad<ADS.length?'$'+adP():'MAX']);
 rows.push(['7. 👟 Скорость ур.'+spd,spd>=SPD.length?'MAX':'$'+spP()]);
 if(rival){rows.push(['8. 🤢 Режим порчи: '+(spoilOn?'ВКЛ':'выкл'),'25% цены']);rows.push(['9. 💣 Бомба','$20000'])}
 else{rows.push(['8. ⭐ +0.5 звезды',stars>=5?'MAX':'$'+starPrice()]);
 rows.push(['9. 🏁 Закончить игру','5★ и $'+goal()])}
 rows.forEach((r,i)=>{const y=py+29+i*11.5;x.textAlign='left';x.fillText(r[0],px+10,y);x.textAlign='right';x.fillText(r[1],px+w-10,y)});x.textAlign='left'}
function sellPanel(){
 const w=240,h=120,px=L.x+5,py=82;box(px,py,w,h);
 x.font='bold 15px system-ui';x.fillText('Покупатель',px+12,py+22);x.font='14px system-ui';
 if(cust){const it=items[cust.i];
  x.fillText('Хочет: '+it.n+' (у тебя '+inv[cust.i]+(rival?', 🤢'+bad[cust.i]:'')+')',px+12,py+46);
  x.fillText('Заплатит: $'+it.sell+(rival?' (🤢 $'+Math.round(it.sell*.5)+')':''),px+12,py+66);
  x.fillStyle='#444';x.fillRect(px+12,py+78,w-24,8);x.fillStyle=cust.t<6?'#e63946':'#52b788';x.fillRect(px+12,py+78,(w-24)*cust.t/PAT,8);
  x.fillStyle='#bbb';x.font='12px system-ui';
  x.fillText(inside(L)?(rival?'E — продать, F — испорч., Q — отказ':'E — продать, Q — отказать'):'Зайди в магазин: E / Q',px+12,py+106)}
 else if(!has())x.fillText('Нет товара — клиентов нет',px+12,py+50);
 else x.fillText('Следующий через '+Math.max(0,wait).toFixed(1)+' с',px+12,py+50)}
function casinoPanel(){
 const w=240,h=124,px=C.x+5,py=82;box(px,py,w,h);
 x.font='bold 13px system-ui';x.fillText('🎰 Казино   Ставка: $'+bet,px+10,py+16);
 const v=spin?[-2,2][Math.floor(performance.now()/80)%2]:window.lastR;
 x.fillStyle='#222';x.fillRect(px+70,py+24,100,38);x.strokeStyle='#ffd23f';x.lineWidth=3;x.strokeRect(px+70,py+24,100,38);
 x.fillStyle=v==2?'#52b788':v==-2?'#e63946':'#ffd23f';x.font='bold 26px system-ui';x.textAlign='center';
 x.fillText(v===undefined?'X?':'X'+v,px+120,py+52);
 x.textAlign='left';x.fillStyle='#bbb';x.font='11px system-ui';
 x.fillText('1: +10  2: +100  3: +1000  6: +1',px+10,py+78);
 x.fillText('4: МАКС (всё)   5: сброс   E: крутить',px+10,py+92);
 x.fillText(kid?'Детский режим: только X2!':hard?'Hard: X-2 — 2/3 шанса, X2 — 1/3':'X-2: −ставка    X2: +ставка',px+10,py+106);
 x.fillText('Ставка не больше баланса',px+10,py+120)}
function clickPanel(){
 const w=240,h=110,px=K.x+5,py=82;box(px,py,w,h);
 x.font='bold 15px system-ui';x.fillText('🟪 Кликер',px+12,py+22);x.font='14px system-ui';
 x.fillText('Прыгай и бей блок снизу!',px+12,py+44);
 x.fillText('Ударов: '+clicks,px+12,py+64);
 x.fillStyle='#444';x.fillRect(px+12,py+74,w-24,8);x.fillStyle='#ffb703';x.fillRect(px+12,py+74,(w-24)*(clicks%cn())/cn(),8);
 x.fillStyle='#bbb';x.font='12px system-ui';x.fillText(cn()+' ударов = $1  ('+(clicks%cn())+'/'+cn()+')',px+12,py+100)}
function cauldron(){
 const cx=M.x+M.w/2,t=performance.now()/1000;
 x.fillStyle='#222';x.fillRect(cx-34,G-44,68,44);x.beginPath();x.ellipse(cx,G-44,38,9,0,0,7);x.fillStyle='#444';x.fill();
 x.beginPath();x.ellipse(cx,G-44,32,6,0,0,7);x.fillStyle=brewing?'#b455ff':'#7cfc00';x.fill();
 x.fillStyle='#fff9';for(let i=0;i<(brewing?6:2);i++){const a=(t*(brewing?1.6:.6)+i*.37)%1;x.beginPath();x.arc(cx-20+i*8+Math.sin(t*3+i)*3,G-48-a*(brewing?40:14),2+i%3,0,7);x.fill()}}
function magicPanel(){
 const w=240,h=126,px=M.x+5,py=82;box(px,py,w,h);
 x.font='bold 13px system-ui';
 if(pmenu){
  const n8=['1. ⬆ Увеличение','2. ⬇ Уменьшение','3. 👟 Скорость +1','4. 💰 Прибыль +5%','5. 📉 Анти-прибыль −5%','6. 🍀 Удача','7. 🌑 Неудача','8. ☠ Смерть'],list=hw?n8.slice(0,7):n8;
  x.fillText('Выбери эффект (−$'+(kid?1:hw?100:1000)+')',px+10,py+16);x.font='11.5px system-ui';
  list.forEach((t,i)=>x.fillText(t,px+10,py+31+i*11.5));
  x.fillStyle='#bbb';x.font='11px system-ui';x.fillText('Q — отмена',px+10,py+122);return}
 if(hw){
  x.fillText('🧙 Дом магии (Хеллоуин)',px+10,py+16);x.font='12px system-ui';
  x.fillText('1 — выбрать зелье ($100)',px+10,py+34);
  x.fillText(hwLvl<5?'2 — ускорить эффекты: ур.'+(hwLvl+1)+' ($'+HWC[hwLvl]+')':'2 — скорость эффектов: максимум',px+10,py+50);
  x.fillText(hwDeath?'☠ Зелье смерти: включено':'☠ Зелье смерти: выключено',px+10,py+66);
  x.fillStyle='#bbb';x.font='11px system-ui';
  x.fillText('Новый эффект каждые '+HWI[hwLvl]+' с (через '+Math.max(0,Math.ceil(hwT))+' с)',px+10,py+88);
  x.fillText('Случайное зелье купить нельзя',px+10,py+104);
  x.fillText('Зелья действуют 1 минуту',px+10,py+118);return}
 x.fillText('🧙 Дом магии',px+10,py+16);x.font='12px system-ui';
 x.fillText(brewing?'Варится зелье...':'E — случайное зелье ($'+(kid?1:hard?1000:100)+')',px+10,py+36);
 x.fillText(hard?'Выбор зелья недоступен (Hard)':'1 — выбрать эффект ($'+(kid?1:1000)+')',px+10,py+52);
 x.fillStyle='#bbb';x.font='11px system-ui';
 x.fillText('⬆ рост ⬇ уменьш. 👟 скор. 🍀 удача',px+10,py+72);
 x.fillText('💰 приб. 📉 анти 🌑 неудача ☠ смерть',px+10,py+86);
 x.fillText(hard?'Шанс: ☠ ×3, ⬇ и 📉 ×2':'Все зелья, кроме смерти — 1 минуту',px+10,py+102);
 x.fillText('Зелье смерти заканчивает игру!',px+10,py+116)}
function customer(){
 if(!cust)return;const cx=L.x+24;
 csq(x,cx,G-28,cust.col,cust.t/PAT>0.6?2:cust.t/PAT>0.3?1:0);
 x.fillStyle='#fff';x.beginPath();x.arc(cx+14,G-62,16,0,7);x.fill();
 item(cust.i,cx+14,G-62,7)}
/* ===== НОВАЯ ГРАФИКА ===== */
let gfxOld=false;try{gfxOld=localStorage.getItem('shopgfx')==='old'}catch(e){}
const mkC=(w,h)=>{const c=document.createElement('canvas');c.width=w;c.height=h;return c};
const rngF=seed=>{let a=seed>>>0;return()=>{a=(Math.imul(a,1664525)+1013904223)>>>0;return a/4294967296}};
const rr=(g,a,b,w,h,r)=>{g.beginPath();if(g.roundRect)g.roundRect(a,b,w,h,r);else g.rect(a,b,w,h)};
const inner=(g,s,f)=>{g.save();g.beginPath();g.rect(s.x,ROOF+10,s.w,G-ROOF-10);g.clip();f();g.restore()};
function itemG(g,i,px,py,s){
 g.save();g.lineJoin='round';
 if(i==0){g.fillStyle='#d62828';g.strokeStyle='#7a1010';g.lineWidth=1;g.beginPath();g.arc(px,py+s*.1,s,0,7);g.fill();g.stroke();
  g.fillStyle='#4caf50';g.beginPath();g.ellipse(px+s*.4,py-s*.95,s*.45,s*.22,-.6,0,7);g.fill();
  g.strokeStyle='#5a3a1a';g.lineWidth=1.3;g.beginPath();g.moveTo(px,py-s*.8);g.lineTo(px-s*.1,py-s*1.3);g.stroke();
  g.fillStyle='rgba(255,255,255,.5)';g.beginPath();g.arc(px-s*.4,py-s*.2,s*.22,0,7);g.fill()}
 else if(i==1){g.fillStyle='#ff8a1f';g.strokeStyle='#a84d00';g.lineWidth=1;g.beginPath();g.moveTo(px-s*.7,py-s*.7);g.lineTo(px+s*.7,py-s*.7);g.lineTo(px,py+s*1.4);g.closePath();g.fill();g.stroke();
  g.strokeStyle='#c96a10';g.beginPath();g.moveTo(px-s*.3,py-s*.1);g.lineTo(px+s*.1,py-s*.1);g.moveTo(px-s*.15,py+s*.5);g.lineTo(px+s*.2,py+s*.5);g.stroke();
  g.fillStyle='#3fae3f';[-.5,0,.5].forEach(d=>{g.beginPath();g.ellipse(px+d*s,py-s*1.1,s*.2,s*.5,d*.7,0,7);g.fill()})}
 else if(i==2){g.fillStyle='#fff';g.strokeStyle='#5c7a99';g.lineWidth=1;g.fillRect(px-s*.7,py-s*.8,s*1.4,s*1.8);g.strokeRect(px-s*.7,py-s*.8,s*1.4,s*1.8);
  g.fillStyle='#4aa3ff';g.fillRect(px-s*.7,py-s*.1,s*1.4,s*.7);
  g.fillStyle='#e8f3ff';g.beginPath();g.moveTo(px-s*.7,py-s*.8);g.lineTo(px,py-s*1.35);g.lineTo(px+s*.7,py-s*.8);g.closePath();g.fill();g.stroke()}
 else if(i==3){g.fillStyle='#ffd23f';g.strokeStyle='#b8860b';g.lineWidth=1;g.beginPath();g.moveTo(px-s,py+s*.7);g.lineTo(px+s,py+s*.7);g.lineTo(px+s,py-s*.3);g.lineTo(px-s,py-s*.9);g.closePath();g.fill();g.stroke();
  g.fillStyle='#e6a800';[[-.3,.2,.2],[.4,0,.15],[.45,.45,.12]].forEach(([a,b,r])=>{g.beginPath();g.arc(px+a*s,py+b*s,r*s,0,7);g.fill()})}
 else{g.strokeStyle='#0a6a8a';g.lineWidth=1;g.fillStyle='#4de1ff';g.beginPath();g.moveTo(px,py-s*1.5);g.lineTo(px+s*.32,py-s*1.1);g.lineTo(px+s*.32,py+s*.55);g.lineTo(px-s*.32,py+s*.55);g.lineTo(px-s*.32,py-s*1.1);g.closePath();g.fill();g.stroke();
  g.fillStyle='rgba(255,255,255,.6)';g.fillRect(px-s*.12,py-s*1.1,s*.12,s*1.5);
  g.fillStyle='#7a4a1a';g.fillRect(px-s*.8,py+s*.55,s*1.6,s*.28);g.fillRect(px-s*.14,py+s*.8,s*.28,s*.6)}
 g.restore()}
function item(i,px,py,s){if(gfxOld)itemOld(i,px,py,s);else itemG(x,i,px,py,s*1.15)}
function wallTile(base,b1,b2){const c=mkC(36,60),g=c.getContext('2d');g.fillStyle=base;g.fillRect(0,0,36,60);g.lineCap='round';
 [[10,b1,8],[27,b2,6]].forEach(([cx,col,w])=>{g.strokeStyle=col;g.lineWidth=w;g.beginPath();for(let y=-4;y<=64;y+=2){const xx=cx+4*Math.sin(y/60*Math.PI*2);y==-4?g.moveTo(xx,y):g.lineTo(xx,y)}g.stroke()});
 g.fillStyle='rgba(255,255,255,.55)';[[18,10],[3,40],[32,28]].forEach(([a,b])=>{g.beginPath();g.arc(a,b,1.6,0,7);g.fill()});return c}
function stoneRect(g,x0,y0,w,h,c1,c2,line,seed){
 const gr=g.createLinearGradient(0,y0,0,y0+h);gr.addColorStop(0,c1);gr.addColorStop(1,c2);g.fillStyle=gr;g.fillRect(x0,y0,w,h);
 const r=rngF(seed);g.strokeStyle=line;g.lineWidth=1;g.save();g.beginPath();g.rect(x0,y0,w,h);g.clip();
 for(let i=0;i<w/8;i++){const a=x0+r()*w,b=y0+r()*h;g.beginPath();g.moveTo(a,b);g.lineTo(a+6+r()*10,b+(r()-.5)*8);g.lineTo(a+10+r()*10,b+(r()-.5)*10);g.stroke()}g.restore()}
function cobble(g,x0,y0,w,h){
 g.save();g.beginPath();g.rect(x0,y0,w,h);g.clip();g.fillStyle='#2b2b30';g.fillRect(x0,y0,w,h);const r=rngF(x0+7);
 for(let row=0;row*9<h;row++){let xx=x0-(row%2?10:0);while(xx<x0+w){const ww=14+r()*10;g.fillStyle='hsl(240,4%,'+(27+r()*10)+'%)';rr(g,xx+1,y0+row*9+1,ww-2,7,3);g.fill();xx+=ww}}g.restore()}
function pillars(g,s,c1){const w=12;let gr=g.createLinearGradient(s.x,0,s.x+w,0);gr.addColorStop(0,c1);gr.addColorStop(1,'rgba(0,0,0,0)');g.fillStyle=gr;g.fillRect(s.x,ROOF+10,w,110);
 gr=g.createLinearGradient(s.x+s.w,0,s.x+s.w-w,0);gr.addColorStop(0,c1);gr.addColorStop(1,'rgba(0,0,0,0)');g.fillStyle=gr;g.fillRect(s.x+s.w-w,ROOF+10,w,110)}
function lamp(g,cx,y0,glow){
 g.strokeStyle='#1d1d1d';g.lineWidth=1.5;g.fillStyle='#e4e4e4';g.beginPath();g.moveTo(cx-15,y0);g.lineTo(cx+15,y0);g.lineTo(cx+10,y0+12);g.lineTo(cx-10,y0+12);g.closePath();g.fill();g.stroke();
 g.fillStyle='#fff';g.fillRect(cx-9,y0+8,18,3);
 const gr=g.createRadialGradient(cx,y0+12,2,cx,y0+12,52);gr.addColorStop(0,glow||'rgba(255,255,235,.5)');gr.addColorStop(1,'rgba(255,255,235,0)');g.fillStyle=gr;g.fillRect(cx-54,y0+8,108,96)}
function table(g,tx,gy){
 const w=140,ty=gy-36;g.fillStyle='rgba(0,0,0,.28)';g.fillRect(tx-3,gy-3,w+6,3);
 const gr=g.createLinearGradient(0,ty,0,gy);gr.addColorStop(0,'#dcdcdc');gr.addColorStop(1,'#9a9a9a');g.fillStyle=gr;g.fillRect(tx,ty+6,w,30);
 g.strokeStyle='#4a4a4a';g.lineWidth=2;g.strokeRect(tx,ty+6,w,30);
 for(let i=0;i<5;i++){g.fillStyle='#6a6a6a';g.fillRect(tx+6+i*27,ty+13,19,17);g.fillStyle='#c4c4c4';g.fillRect(tx+8+i*27,ty+15,15,13)}
 g.fillStyle='#ececec';g.fillRect(tx-4,ty,w+8,7);g.strokeRect(tx-4,ty,w+8,7);
 for(let i=0;i<5;i++)itemG(g,i,tx+14+i*28,ty-9,8)}
function counter(g,cx,gy){
 const w=64,top=gy-28;g.fillStyle='#f2f2f2';g.fillRect(cx,top,w,28);g.strokeStyle='#555';g.lineWidth=2;g.strokeRect(cx,top,w,28);
 g.fillStyle='#222';g.fillRect(cx-3,top-4,w+6,5);g.fillStyle='#8a8a8a';
 for(let i=0;i<5;i++){g.beginPath();g.moveTo(cx+28+i*8,top-4);g.lineTo(cx+34+i*8,top-4);g.lineTo(cx+30+i*8,top+1);g.lineTo(cx+24+i*8,top+1);g.fill()}
 g.fillStyle='#111';g.fillRect(cx+24,top-26,12,3);g.fillRect(cx+20,top-23,20,21);g.fillStyle='#ddd';g.fillRect(cx+25,top-16,10,3);g.fillStyle='#333';g.fillRect(cx+20,top-9,20,3)}
function ceilSlab(g,s,c1,c2,line,seed){stoneRect(g,s.x-8,ROOF-10,s.w+16,20,c1,c2,line,seed);g.fillStyle='rgba(0,0,0,.4)';g.fillRect(s.x-8,ROOF+8,s.w+16,2)}
function roofSign(g,s,name,c1,c2){
 const w=s.w-100,xx=s.x+50,yy=ROOF-34;g.fillStyle='#2b2b2b';g.fillRect(xx+8,yy+20,5,12);g.fillRect(xx+w-13,yy+20,5,12);
 const gr=g.createLinearGradient(0,yy,0,yy+24);gr.addColorStop(0,c1);gr.addColorStop(1,c2);g.fillStyle=gr;rr(g,xx,yy,w,24,6);g.fill();g.strokeStyle='#111';g.lineWidth=2;g.stroke();
 g.fillStyle='#fff';g.font='bold 14px system-ui';g.textAlign='center';g.shadowColor='#000';g.shadowBlur=3;g.fillText(name,s.x+s.w/2,yy+17);g.shadowBlur=0}
function drawBuy(g,s,nm){
 inner(g,s,()=>{g.fillStyle=g.createPattern(wallTile('#edf3ee','#c5d3c7','#abbcae'),'repeat');g.fillRect(s.x,ROOF+10,s.w,110);
  pillars(g,s,'#3a4a3f');cobble(g,s.x,G-16,s.w,16);lamp(g,s.x+62,ROOF+10);lamp(g,s.x+190,ROOF+10);counter(g,s.x+14,G);table(g,s.x+s.w-22-140,G)});
 ceilSlab(g,s,'#4a4a4e','#2c2c30','#1d1d20',11);roofSign(g,s,nm||'ПОКУПКА','#3fa66a','#1f6b3f')}
function drawSell(g,s,nm){
 inner(g,s,()=>{g.fillStyle=g.createPattern(wallTile('#f8f1e4','#e4d4b6','#ceb88f'),'repeat');g.fillRect(s.x,ROOF+10,s.w,110);
  pillars(g,s,'#52412e');cobble(g,s.x,G-16,s.w,16);lamp(g,s.x+80,ROOF+10,'rgba(255,225,170,.5)');lamp(g,s.x+200,ROOF+10,'rgba(255,225,170,.5)');
  g.fillStyle='#b3262e';g.fillRect(s.x+14,G-6,70,6);g.fillStyle='#e8c25a';for(let i=0;i<8;i++)g.fillRect(s.x+16+i*9,G-6,3,6);
  g.fillStyle='#7a4a1a';g.fillRect(s.x+26,ROOF+36,34,24);g.fillStyle='#bfe9ff';g.fillRect(s.x+29,ROOF+39,28,18);g.strokeStyle='#fff';g.lineWidth=1.5;g.beginPath();g.moveTo(s.x+43,ROOF+39);g.lineTo(s.x+43,ROOF+57);g.moveTo(s.x+29,ROOF+48);g.lineTo(s.x+57,ROOF+48);g.stroke();
  table(g,s.x+s.w-22-140,G)});
 ceilSlab(g,s,'#524a44','#2f2a26','#211d1a',23);roofSign(g,s,nm||'ПРОДАЖА','#e8892b','#a84f0a')}
function drawCasino(g,s){
 inner(g,s,()=>{for(let y=ROOF+10,k=0;y<G;y+=6,k++){g.fillStyle=k%2?'#f7f23a':'#fffb70';g.fillRect(s.x,y,s.w,6)}
  [s.x,s.x+s.w-14].forEach((xx,j)=>{g.fillStyle='#6b3b1a';g.fillRect(xx,ROOF+10,14,110);g.strokeStyle='#3d1f0a';g.lineWidth=1;for(let k=4;k<14;k+=5){g.beginPath();g.moveTo(xx+k,ROOF+10);g.lineTo(xx+k,G);g.stroke()}g.fillStyle='rgba(0,0,0,.28)';g.fillRect(xx+(j?0:10),ROOF+10,4,110)});
  g.fillStyle='#5a0a0a';g.fillRect(s.x,G-16,s.w,16);g.strokeStyle='#7d1a1a';g.lineWidth=1;for(let xx=s.x;xx<s.x+s.w;xx+=18){g.beginPath();g.moveTo(xx,G);g.lineTo(xx+9,G-14);g.lineTo(xx+18,G);g.stroke()}
  const cx=s.x+s.w/2;g.fillStyle='#ffff6a';g.strokeStyle='#000';g.lineWidth=2;g.beginPath();g.ellipse(cx,G-80,34,19,0,Math.PI,0);g.closePath();g.fill();g.stroke();
  g.beginPath();g.moveTo(cx,G-99);g.lineTo(cx,G-80);g.moveTo(cx-22,G-92);g.lineTo(cx-4,G-80);g.moveTo(cx+22,G-92);g.lineTo(cx+4,G-80);g.stroke();
  const gr=g.createRadialGradient(cx,G-45,6,cx,G-45,52);gr.addColorStop(0,'#ff1a1a');gr.addColorStop(1,'#8a0000');g.fillStyle=gr;g.fillRect(cx-34,G-80,68,80);g.strokeRect(cx-34,G-80,68,80);
  const sg=g.createLinearGradient(0,G-66,0,G-40);sg.addColorStop(0,'#fff');sg.addColorStop(1,'#cfcfcf');g.fillStyle=sg;g.fillRect(cx-26,G-66,52,26);g.lineWidth=2.5;g.strokeRect(cx-26,G-66,52,26);
  g.fillStyle='#ececec';g.fillRect(cx-20,G-30,40,10);g.lineWidth=2;g.strokeRect(cx-20,G-30,40,10)});
 g.fillStyle='#050505';g.fillRect(s.x-8,ROOF-10,s.w+16,20);g.fillStyle='#2a2a2a';g.fillRect(s.x-8,ROOF-10,s.w+16,2);roofSign(g,s,'КАЗИНО','#d4202a','#7a0a10')}
function pBar(g,x0,y0,w,h){const gr=g.createLinearGradient(0,y0,0,y0+h);gr.addColorStop(0,'#8a3cff');gr.addColorStop(1,'#5316d8');g.fillStyle=gr;g.fillRect(x0,y0,w,h);g.fillStyle='#c9a2ff';g.fillRect(x0,y0,w,1.5);g.fillStyle='#3a0fa8';for(let xx=x0+10;xx<x0+w-4;xx+=28){g.beginPath();g.ellipse(xx,y0+h/2,3,2,0,0,7);g.fill()}}
function wavyPillar(g,x0,w,c1,c2){g.fillStyle=c1;g.fillRect(x0,ROOF+10,w,110);g.strokeStyle=c2;g.lineWidth=3;g.beginPath();for(let y=ROOF+10;y<=G;y+=3){const xx=x0+w/2+3.2*Math.sin(y/9);y===ROOF+10?g.moveTo(xx,y):g.lineTo(xx,y)}g.stroke()}
function drawClick(g,s){
 inner(g,s,()=>{const cx=s.x+s.w/2,rg=g.createRadialGradient(cx,ROOF+70,8,cx,ROOF+70,160);rg.addColorStop(0,'#ff8cff');rg.addColorStop(1,'#ff10e8');g.fillStyle=rg;g.fillRect(s.x,ROOF+10,s.w,110);
  g.strokeStyle='rgba(255,255,255,.2)';g.lineWidth=1;for(let k=-120;k<s.w+120;k+=34){g.beginPath();g.moveTo(s.x+k,ROOF+10);g.lineTo(s.x+k+110,G);g.moveTo(s.x+k+110,ROOF+10);g.lineTo(s.x+k,G);g.stroke()}
  wavyPillar(g,s.x,14,'#6b1fe0','#b36bff');wavyPillar(g,s.x+s.w-14,14,'#6b1fe0','#b36bff');pBar(g,s.x,G-12,s.w,12)});
 pBar(g,s.x-8,ROOF-10,s.w+16,20);g.fillStyle='rgba(0,0,0,.35)';g.fillRect(s.x-8,ROOF+8,s.w+16,2);roofSign(g,s,'КЛИКЕР','#c026ff','#6a14c2')}
function bottle(g,bx,by,col){g.fillStyle=col;g.strokeStyle='rgba(0,0,0,.55)';g.lineWidth=1;rr(g,bx,by,9,14,3);g.fill();g.stroke();g.fillStyle='#d9c9a0';g.fillRect(bx+2.5,by-4,4,5);g.fillStyle='rgba(255,255,255,.5)';g.fillRect(bx+2,by+3,2,7)}
function orb(g,cx,cy){const gr=g.createRadialGradient(cx,cy,1,cx,cy,34);gr.addColorStop(0,'rgba(236,200,255,.95)');gr.addColorStop(.35,'rgba(176,96,255,.5)');gr.addColorStop(1,'rgba(176,96,255,0)');g.fillStyle=gr;g.fillRect(cx-36,cy-36,72,72);g.fillStyle='#fff';g.beginPath();g.arc(cx,cy,3,0,7);g.fill()}
function drawMagic(g,s){
 inner(g,s,()=>{const wg=g.createLinearGradient(0,ROOF+10,0,G);wg.addColorStop(0,'#2d1853');wg.addColorStop(1,'#170a30');g.fillStyle=wg;g.fillRect(s.x,ROOF+10,s.w,110);
  g.strokeStyle='rgba(180,107,255,.16)';g.lineWidth=1;for(let k=-120;k<s.w+120;k+=34){g.beginPath();g.moveTo(s.x+k,ROOF+10);g.lineTo(s.x+k+110,G);g.moveTo(s.x+k+110,ROOF+10);g.lineTo(s.x+k,G);g.stroke()}
  const r=rngF(5);for(let i=0;i<30;i++){g.fillStyle='rgba(235,205,255,'+(.3+r()*.6)+')';const a=s.x+16+r()*(s.w-32),b=ROOF+14+r()*70;g.fillRect(a,b,2,2);if(i%5==0){g.fillRect(a-2,b+.5,6,1);g.fillRect(a+.5,b-2,1,6)}}
  wavyPillar(g,s.x,14,'#3a1670','#7a3fd0');wavyPillar(g,s.x+s.w-14,14,'#3a1670','#7a3fd0');
  g.fillStyle='#120726';g.fillRect(s.x,G-14,s.w,14);g.strokeStyle='#3a1a66';g.lineWidth=1;for(let xx=s.x+6;xx<s.x+s.w;xx+=22){g.beginPath();g.moveTo(xx,G);g.lineTo(xx+5,G-6);g.lineTo(xx+2,G-13);g.stroke()}
  [[s.x+20,ROOF+56],[s.x+s.w-72,ROOF+56],[s.x+20,ROOF+86],[s.x+s.w-72,ROOF+86]].forEach(([a,b],i)=>{g.fillStyle='#3a2058';g.fillRect(a,b,52,4);g.fillStyle='#1d0e38';g.fillRect(a,b+4,52,2);
   [['#ff4d6d','#4dff88','#4da3ff'],['#ffd23f','#c06bff','#39ff14']][i%2].forEach((c,j)=>bottle(g,a+6+j*16,b-14,c))});
  orb(g,s.x+50,ROOF+30);orb(g,s.x+s.w-50,ROOF+30)});
 stoneRect(g,s.x-8,ROOF-10,s.w+16,20,'#2a1250','#12062a','#4a2a80',31);g.fillStyle='rgba(0,0,0,.4)';g.fillRect(s.x-8,ROOF+8,s.w+16,2);
 g.strokeStyle='#c58bff';g.lineWidth=1.2;for(let i=0;i<4;i++){const cx=s.x+32+i*62;g.beginPath();g.arc(cx,ROOF,4.5,0,7);g.moveTo(cx-6,ROOF);g.lineTo(cx+6,ROOF);g.moveTo(cx,ROOF-6);g.lineTo(cx,ROOF+6);g.stroke()}
 roofSign(g,s,'ДОМ МАГИИ','#6a2fb5','#2e1260')}
function lampPost(g,px){g.fillStyle='#2a2a2e';g.fillRect(px-2,G-78,4,78);g.fillRect(px-8,G-4,16,4);g.beginPath();g.moveTo(px-12,G-78);g.lineTo(px+12,G-78);g.lineTo(px+8,G-90);g.lineTo(px-8,G-90);g.closePath();g.fill();
 const gr=g.createRadialGradient(px,G-80,2,px,G-80,36);gr.addColorStop(0,'rgba(255,240,150,.75)');gr.addColorStop(1,'rgba(255,240,150,0)');g.fillStyle=gr;g.fillRect(px-38,G-118,76,76);g.fillStyle='#fff6b0';g.fillRect(px-6,G-80,12,3)}
function bushG(g,px,sc){[[-14,0,16],[0,-6,20],[14,0,16]].forEach(([a,b,r],i)=>{g.fillStyle=i==1?'#168a2c':'#0f6b22';g.beginPath();g.arc(px+a*sc,G-r*sc*.7+b*sc,r*sc,0,7);g.fill()});g.fillStyle='rgba(120,255,140,.25)';g.beginPath();g.arc(px-4*sc,G-22*sc,6*sc,0,7);g.fill()}
function buildWorld(){
 const c=mkC(WW,420),g=c.getContext('2d');
 const gg=g.createLinearGradient(0,G,0,420);gg.addColorStop(0,'#cdb482');gg.addColorStop(.5,'#9f8a5e');gg.addColorStop(1,'#5a4b33');g.fillStyle=gg;g.fillRect(0,G,WW,60);g.fillStyle='#e8d6a6';g.fillRect(0,G,WW,2);
 const r=rngF(99);g.strokeStyle='rgba(60,45,20,.3)';g.lineWidth=1.2;for(let i=0;i<WW/24;i++){const a=r()*WW,b=G+6+r()*48;g.beginPath();g.moveTo(a,b);g.lineTo(a+8+r()*14,b-6+r()*12);g.lineTo(a+18+r()*18,b-8+r()*16);g.stroke()}
 g.strokeStyle='rgba(60,45,20,.18)';for(let xx=0;xx<WW;xx+=64){g.beginPath();g.moveTo(xx,G+2);g.lineTo(xx-10,420);g.stroke()}
 [335,565,900,1210,1520].forEach(p=>lampPost(g,p));bushG(g,450,1);bushG(g,1790,1);
 g.fillStyle='#6b3f1d';g.fillRect(400,G-16,100,5);g.fillRect(404,G-11,5,11);g.fillRect(491,G-11,5,11);g.fillStyle='#8a5a2b';g.fillRect(400,G-30,100,4);
 drawBuy(g,L,'ПРОДАЖА');drawSell(g,R,'ПОКУПКА');drawCasino(g,C);drawClick(g,K);drawMagic(g,M);return c}
function cloud(g,cx,cy,s){[[0,5,40,11,'#dcf1ff'],[0,0,40,12,'#fff'],[-22,-5,22,10,'#fff'],[18,-8,26,12,'#fff'],[-4,-11,20,10,'#fff']].forEach(([a,b,rx,ry,col])=>{g.fillStyle=col;g.beginPath();g.ellipse(cx+a*s,cy+b*s,rx*s,ry*s,0,0,7);g.fill()})}
function buildCity(){
 const W=1400,c=mkC(W,420),g=c.getContext('2d'),r=rngF(20261017);
 const sky=g.createLinearGradient(0,0,0,360);sky.addColorStop(0,'#1fdaee');sky.addColorStop(1,'#92f8ff');g.fillStyle=sky;g.fillRect(0,0,W,420);
 for(let i=0;i<14;i++){const gx=r()*W,gy=r()*260,rg=g.createRadialGradient(gx,gy,0,gx,gy,80+r()*60);rg.addColorStop(0,'rgba(255,255,255,.2)');rg.addColorStop(1,'rgba(255,255,255,0)');g.fillStyle=rg;g.fillRect(gx-150,gy-150,300,300)}
 for(let i=0;i<14;i++)cloud(g,40+i*100+r()*40,24+r()*150,.65+r()*.7);
 const cols=[['#8aa0c4','#5d6f9c'],['#4f7fbf','#34569a'],['#7f94a6','#566177'],['#7fb0d6','#4f84ad'],['#9aa3c8','#6d759f']];
 let bx=-30;while(bx<W){const w=100+r()*90,h=150+r()*140,cc=cols[Math.floor(r()*cols.length)],top=352-h,wf=w*.72,ws=w-wf;
  g.fillStyle=cc[0];g.fillRect(bx,top,wf,h);g.fillStyle=cc[1];g.beginPath();g.moveTo(bx+wf,top);g.lineTo(bx+w,top+12);g.lineTo(bx+w,352);g.lineTo(bx+wf,352);g.closePath();g.fill();
  g.fillStyle='rgba(255,255,255,.15)';g.beginPath();g.moveTo(bx,top);g.lineTo(bx+wf,top);g.lineTo(bx+w,top+12);g.lineTo(bx+wf,top+4);g.closePath();g.fill();
  for(let cx=bx+8;cx<bx+wf-10;cx+=16)for(let cy=top+12;cy<340;cy+=26){g.fillStyle=r()<.2?'#6fdcee':'#a9f8ff';g.fillRect(cx,cy,8,14);g.fillStyle='rgba(255,255,255,.5)';g.fillRect(cx+1,cy+1,2,12)}
  bx+=w*.78+r()*30}
 g.fillStyle='#0b4f20';g.fillRect(0,352,W,12);
 for(let pass=0;pass<2;pass++)for(let xx=-10;xx<W+10;xx+=8+r()*8){const h=(pass?10:14)+r()*(pass?16:22),w=9+r()*6;g.fillStyle=pass?(r()<.5?'#0a7a1e':'#16a032'):'#0d5f27';g.beginPath();g.moveTo(xx,364);g.lineTo(xx+w/2,364-h);g.lineTo(xx+w,364);g.closePath();g.fill()}
 return c}
let cityCv=null,worldCv=null;try{cityCv=buildCity();worldCv=buildWorld()}catch(e){cityCv=worldCv=null}
function casinoScreen(){const cx=C.x+C.w/2,v=spin?[-2,2][Math.floor(performance.now()/80)%2]:window.lastR;x.font='bold 20px system-ui';x.textAlign='center';x.fillStyle=v===2?'#12a150':v===-2?'#e0202f':'#555';x.fillText(v===undefined?'X?':'X'+v,cx,G-47)}
function clickerFrac(){const b=BLK,by=b.y-(bump>0?4:0),cx=b.x+b.w+16,n=cn(),a=String(clicks%n);x.font='bold 13px system-ui';x.textAlign='center';x.lineWidth=3;x.strokeStyle='#000';x.fillStyle='#fff';
 x.strokeText(a,cx,by+12);x.fillText(a,cx,by+12);x.fillRect(cx-10,by+15,20,1.6);x.strokeText(String(n),cx,by+28);x.fillText(String(n),cx,by+28)}
function drawBlock(by){const b=BLK,g=x,bv=5;g.fillStyle='#d915ff';g.fillRect(b.x,by,b.w,b.h);
 const poly=(col,pts)=>{g.fillStyle=col;g.beginPath();pts.forEach((p,i)=>i?g.lineTo(p[0],p[1]):g.moveTo(p[0],p[1]));g.closePath();g.fill()};
 poly('#f27bff',[[b.x,by],[b.x+b.w,by],[b.x+b.w-bv,by+bv],[b.x+bv,by+bv]]);poly('#ff9bff',[[b.x,by],[b.x+bv,by+bv],[b.x+bv,by+b.h-bv],[b.x,by+b.h]]);
 poly('#8d00b3',[[b.x,by+b.h],[b.x+b.w,by+b.h],[b.x+b.w-bv,by+b.h-bv],[b.x+bv,by+b.h-bv]]);poly('#a600cf',[[b.x+b.w,by],[b.x+b.w,by+b.h],[b.x+b.w-bv,by+b.h-bv],[b.x+b.w-bv,by+bv]]);
 const rg=g.createRadialGradient(b.x+b.w/2,by+b.h/2,1,b.x+b.w/2,by+b.h/2,16);rg.addColorStop(0,'rgba(255,200,255,.5)');rg.addColorStop(1,'rgba(255,200,255,0)');g.fillStyle=rg;g.fillRect(b.x+bv,by+bv,b.w-2*bv,b.h-2*bv);
 g.strokeStyle='rgba(60,0,80,.8)';g.lineWidth=1.5;g.strokeRect(b.x,by,b.w,b.h)}
function cauldronNew(){
 const g=x,cx=M.x+M.w/2,t=performance.now()/1000,col1=brewing?'#c26bff':'#6dff3a',col2=brewing?'#7a1fd0':'#1fa81f';
 const f=g.createRadialGradient(cx,G-4,2,cx,G-4,50);f.addColorStop(0,'rgba(255,150,40,.6)');f.addColorStop(1,'rgba(255,100,0,0)');g.fillStyle=f;g.fillRect(cx-54,G-54,108,54);
 g.fillStyle='#0d0d10';g.fillRect(cx-28,G-10,7,10);g.fillRect(cx+21,G-10,7,10);
 const bg=g.createRadialGradient(cx-12,G-36,4,cx,G-28,44);bg.addColorStop(0,'#53535f');bg.addColorStop(1,'#14141a');g.fillStyle=bg;
 g.beginPath();g.moveTo(cx-36,G-46);g.bezierCurveTo(cx-46,G-10,cx-24,G-6,cx,G-6);g.bezierCurveTo(cx+24,G-6,cx+46,G-10,cx+36,G-46);g.closePath();g.fill();g.strokeStyle='#000';g.lineWidth=2;g.stroke();
 g.lineWidth=3;g.beginPath();g.arc(cx-40,G-38,6,Math.PI*.5,Math.PI*1.5);g.stroke();g.beginPath();g.arc(cx+40,G-38,6,-Math.PI*.5,Math.PI*.5);g.stroke();
 g.fillStyle='#2c2c36';g.beginPath();g.ellipse(cx,G-46,38,9,0,0,7);g.fill();g.lineWidth=2;g.stroke();
 const lg=g.createLinearGradient(0,G-52,0,G-40);lg.addColorStop(0,col1);lg.addColorStop(1,col2);g.fillStyle=lg;g.beginPath();g.ellipse(cx,G-46,32,6.5,0,0,7);g.fill();
 const n=brewing?7:3;for(let i=0;i<n;i++){const a=(t*(brewing?1.5:.55)+i*.31)%1;g.fillStyle='rgba(255,255,255,'+(.75*(1-a))+')';g.beginPath();g.arc(cx-22+i*7+Math.sin(t*3+i)*3,G-48-a*(brewing?44:18),1.8+i%3,0,7);g.fill()}
 for(let i=0;i<5;i++){const a=(t*.35+i*.2)%1,sx=cx-30+i*15+Math.sin(t*1.6+i*2)*5,sy=G-60-a*38;g.fillStyle='rgba(235,200,255,'+(.8*(1-a))+')';g.fillRect(sx-1,sy-3,2,6);g.fillRect(sx-3,sy-1,6,2)}}

/* ===== РЕЖИМ С ДЕБАФАМИ ===== */
let deb=false,debMode='min',debList=[],debReveal=null,debT=0,potCnt=0,casCnt=0,perfSales=0,outT=0,shopT=0,lookT=0;
const DEBN=['','Аллергия на воздух','Аллергия на яблоки','Аллергия на морковку','Аллергия на молоко','Аллергия на сыр','Боязнь орудий','Боязнь людей','Боязнь проиграть','Хрупкий череп','Аллергия на зелья','Пристрастный к зельям','Пристрастный к казино','Гигантизм','Карликовость','Неудачник','Толстый','Любитель яблок','Перфекционист','Любитель поесть','Всеминенавистник','Боязнь магазинов','Сегодня без чаевых','Призрак','Лентяй','Неизвестный дебаф'];
const DEBD=['','Нельзя быть вне зданий 3 с подряд','Нельзя покупать яблоки','Нельзя покупать морковку','Нельзя покупать молоко','Нельзя покупать сыр','Нельзя покупать меч','Не смотри на клиента дольше 3 с','Нельзя проигрывать в казино','Нельзя бить блок в кликере','Нельзя пить зелья','Выпей минимум 1 зелье за минуту','Сыграй в казино 1+ раз за минуту','Ты ×8 размера. Нельзя уменьшаться','Ты ×1/8 размера. Нельзя увеличиваться','Нет побед в казино и хороших зелий','Нельзя прыгать','Можно покупать только яблоки','Отказывай хотя бы раз в 10 продаж','25% что съешь купленный товар','Клиенты ждут всего 1 секунду','Нельзя быть в магазине дольше 3 с','Чаевых нет','Ты невидим','Товары — только с доставкой','Правило скрыто'];
const debActive=id=>deb&&debList.some(d=>d.real===id);
function debFail(id){if(over)return;const d=debList.find(q=>q.real===id);lose('Нарушен дебаф: '+DEBN[id]+(d&&d.unk?' (это был неизвестный дебаф!)':''));say('⚠ Дебаф нарушен!')}
function debBuy(ix,it){
 if(!deb)return;
 if(debActive(2+ix))debFail(2+ix);
 else if(debActive(17)&&ix!=0)debFail(17);
 else if(debActive(24)&&inside(R))debFail(24);
 else if(debActive(19)&&Math.random()<.25&&inv[ix]>0){inv[ix]--;say('🍴 Ты съел: '+it.n+'!')}}
function debPick(){
 const N=debMode==='all'?10:3;
 const okc=a=>{const h=i=>a.includes(i);return!((h(10)&&h(11))||(h(13)&&h(14))||(h(2)&&h(17))||(h(8)&&h(12)&&h(15))||(h(2)&&h(3)&&h(4)&&h(5)&&h(6)))};
 for(let tries=0;tries<500;tries++){
  const pool=[];for(let i=1;i<=25;i++)pool.push(i);const pick=[];
  while(pick.length<N)pick.push(pool.splice(Math.floor(Math.random()*pool.length),1)[0]);
  const real=pick.slice(),u=pick.indexOf(25);
  if(u>=0){const rest=[];for(let i=1;i<=24;i++)if(!pick.includes(i))rest.push(i);real[u]=rest[Math.floor(Math.random()*rest.length)]}
  if(okc(real)){debList=pick.map((id,k)=>({id,real:real[k],unk:id===25}));break}}
 potCnt=casCnt=perfSales=0;outT=shopT=lookT=0;PAT=debActive(20)?1:20}
function debTick(dt){
 debT+=dt;if(debReveal){debReveal.t-=dt;if(debReveal.t<=0)debReveal=null}
 const inAny=inside(L)||inside(R)||inside(C)||inside(K)||inside(M);
 outT=inAny?0:outT+dt;shopT=inAny?shopT+dt:0;lookT=(cust&&inside(L))?lookT+dt:0;
 if(debActive(1)&&outT>=3)debFail(1);
 if(debActive(21)&&shopT>=3)debFail(21);
 if(debActive(7)&&lookT>=3)debFail(7);
 if(debT>=60){debT-=60;if(debActive(11)&&potCnt==0)debFail(11);if(debActive(12)&&casCnt==0)debFail(12);
  if(!over){
   if(debMode==='min'){const un=debList.find(d=>d.unk);if(un)debReveal={t:8,txt:DEBN[un.real],d:DEBD[un.real]};debPick();say('🌀 Новые дебафы!'+(un?' Неизвестный был: '+DEBN[un.real]:''))}
   else{potCnt=casCnt=0;if(debActive(11)||debActive(12))say('🌀 Новая минута — проверка обязательств')}}}}
function debStat(r){
 if(r==1&&outT>.05)return 'на улице: '+outT.toFixed(1)+' / 3 с';
 if(r==21&&shopT>.05)return 'в магазине: '+shopT.toFixed(1)+' / 3 с';
 if(r==7&&lookT>.05)return 'смотришь на клиента: '+lookT.toFixed(1)+' / 3 с';
 if(r==11)return 'зелий за минуту: '+potCnt;
 if(r==12)return 'игр в казино за минуту: '+casCnt;
 if(r==18)return 'продаж без отказа: '+perfSales+' / 10';
 return''}
function debShort(r){
 if(r==1&&outT>.05)return outT.toFixed(1)+'/3';
 if(r==21&&shopT>.05)return shopT.toFixed(1)+'/3';
 if(r==7&&lookT>.05)return lookT.toFixed(1)+'/3';
 if(r==11)return String(potCnt);
 if(r==12)return String(casCnt);
 if(r==18)return perfSales+'/10';
 return''}
function drawDeb(){
 const w=232,x0=662,y0=6,cmp=debList.length>3,rh=cmp?15:30,h=24+debList.length*rh;
 x.fillStyle='#000b';x.fillRect(x0,y0,w,h);x.textAlign='left';x.font='bold 12px system-ui';x.fillStyle='#2dd4bf';
 x.fillText(debMode==='all'?'🌀 10 дебафов на всю игру':'🌀 Дебафы · новые через '+fmt(Math.max(0,Math.ceil(60-debT))),x0+8,y0+16);
 debList.forEach((d,i)=>{const yy=y0+24+i*rh,hid=d.unk&&!over,st=hid?'':(cmp?debShort(d.real):debStat(d.real));
  const nm=hid?'❓ '+DEBN[25]:(d.unk?'❓ '+DEBN[d.real]:DEBN[d.id]);
  if(cmp){x.font='bold 11px system-ui';x.fillStyle=st?'#ffcf70':'#fff';x.fillText(nm+(st?' · '+st:''),x0+8,yy+11)}
  else{x.font='bold 12px system-ui';x.fillStyle='#fff';x.fillText(nm,x0+8,yy+10);
   x.font='10.5px system-ui';x.fillStyle=st?'#ffcf70':'#bbb';x.fillText(st||(hid?'Правило скрыто — не нарушай его!':DEBD[d.unk?d.real:d.id]),x0+8,yy+23)}});
 if(debReveal){const ry=y0+h+6;x.fillStyle='#000b';x.fillRect(x0,ry,w,40);x.font='bold 12px system-ui';x.fillStyle='#ffd23f';x.fillText('❓ Неизвестный дебаф был:',x0+8,ry+15);x.fillStyle='#fff';x.fillText(debReveal.txt,x0+8,ry+31)}
 x.textAlign='center'}
function fillDebAll(){debAll.textContent='';for(let i=1;i<=25;i++){const li=document.createElement('li');li.textContent=DEBN[i]+' — '+(i==25?'это может быть любой другой дебаф, но ты не знаешь какой':DEBD[i]);debAll.appendChild(li)}}
fillDebAll();
debBtn.onclick=()=>{debCh.style.display='flex'};
debBtn1.onclick=()=>{debMode='min';reset('d');debCh.style.display='none';altm.style.display='none';menuShow(false)};
debBtn2.onclick=()=>{debMode='all';reset('d');debCh.style.display='none';altm.style.display='none';menuShow(false)};
debChX.onclick=()=>{debCh.style.display='none'};
debInfoBtn.onclick=()=>{debInfo.style.display=debInfo.style.display=='none'?'block':'none'};
rivalBtn.onclick=()=>{rivalIntro.style.display='flex'};
rivalGo.onclick=()=>{reset('r');rivalIntro.style.display='none';altm.style.display='none';menuShow(false)};
rivalX.onclick=()=>{rivalIntro.style.display='none'};
rivalInfoBtn.onclick=()=>{rivalInfo.style.display=rivalInfo.style.display=='none'?'block':'none'};
bf.onclick=sellBad;rs.onclick=restart;
setLabels();try{localStorage.removeItem('shoplang')}catch(e){}
const fmtM=n=>n>=1e15?n.toExponential(2).replace('e+','e'):String(n);
function draw(){
 {const rt=kid?'✨ Возродиться — бесплатно':'✨ Возродиться — $500';if(rvLast!==rt){rvLast=rt;rv.textContent=rt}}rv.style.display=over=='lose'&&kid?'inline-block':'none';rs.style.display=over=='lose'?'inline-block':'none';tl.style.visibility=tr.style.visibility=tj.style.visibility=(over&&!editing)?'hidden':'visible';
 x.clearRect(0,0,900,420);
 const gn=!gfxOld&&cityCv&&worldCv;
 if(gn)x.drawImage(cityCv,-Math.round(cam*0.5),0);else{x.fillStyle='#87c4e8';x.fillRect(0,0,900,420)}
 x.save();x.translate(-Math.round(cam),0);
 if(gn){x.drawImage(worldCv,0,0);casinoScreen();clickerFrac();cauldronNew();customer()}else{
 x.fillStyle='#9aa0a6';x.fillRect(0,G,WW,60);
 x.fillStyle='#c9ced3';x.fillRect(L.x+L.w,G,R.x-L.x-L.w,60);x.fillRect(R.x+R.w,G,C.x-R.x-R.w,60);x.fillRect(C.x+C.w,G,K.x-C.x-C.w,60);x.fillRect(K.x+K.w,G,M.x-K.x-K.w,60);
 x.fillStyle='#fff8';for(let i=300;i<600;i+=40)x.fillRect(i,G+28,22,4);
 shop(L,'ПРОДАЖА','#d9a066');shop(R,'ПОКУПКА','#7fb685');shop(C,'КАЗИНО','#6a4c93',true);shop(K,'КЛИКЕР','#ff9ccf','clk');shop(M,'ДОМ МАГИИ','#5a3d8a','magic');cauldron();customer();}
 
 {const by=BLK.y-(bump>0?4:0);if(gn)drawBlock(by);else{x.fillStyle='#ffb703';x.fillRect(BLK.x,by,BLK.w,BLK.h);x.strokeStyle='#7a4a00';x.lineWidth=3;x.strokeRect(BLK.x,by,BLK.w,BLK.h);x.fillStyle='#7a4a00';x.font='bold 18px system-ui';x.textAlign='center';x.fillText('?',BLK.x+20,by+19)}}
 if(!debActive(23)){x.save();x.translate(p.x,p.y);x.scale(sc,sc);sq(x,0,0,skin.c,skin.e,skin.m,skin.g);x.restore()}
 if(!noPop){sellPanel();if(inside(R))buyPanel();if(inside(C))casinoPanel();if(inside(K))clickPanel();if(inside(M))magicPanel()}
 x.restore();
 x.fillStyle='#000a';x.fillRect(250,6,400,70);x.fillStyle='#fff';x.textAlign='center';x.font='bold 16px system-ui';
 x.fillText('💰 $'+fmtM(money)+(hard?'   🔥 осталось ⏱ '+fmt(Math.max(0,Math.ceil(1500-tm))):(hw?'   🎃 ⏱ ':kid?'   🧒 ⏱ ':deb?'   🌀 ⏱ ':rival?'   🏪 ⏱ ':'   ⏱ ')+fmt(tm))+'   🧾 '+sales,450,26);x.font='13px system-ui';
 x.fillText('🍎'+inv[0]+'  🥕'+inv[1]+'  🥛'+inv[2]+'  🧀'+inv[3]+'  🗡'+inv[4]+'   📢 ур.'+ad+'  👟 ур.'+spLvl(),450,44);
 x.textAlign='left';x.font='12px system-ui';fx.forEach((e,i)=>{x.fillStyle='#000a';x.fillRect(6,6+i*20,190,18);x.fillStyle='#fff';x.fillText(FXN[e.k]+'  '+fmt(Math.ceil(e.t)),12,19+i*20)});if(hw){const i=fx.length;x.fillStyle='#000a';x.fillRect(6,6+i*20,190,18);x.fillStyle='#ffb347';x.fillText('🎃 Новый эффект через '+fmt(Math.ceil(hwT)),12,19+i*20)}if(deb)drawDeb();x.textAlign='center';
 if(rival)drawRating();else{x.fillStyle='#ffd23f';x.fillText('Магазин: '+starTxt()+' | чаевые '+(hard?'нет':(tipRange()[0]+tipBonus())+'…'+(tipRange()[1]+tipBonus())+'%')+' | отказы '+(hard?'1 = конец':rej+'/3'),450,62);}
 x.font='12px system-ui';x.fillStyle='#fff';x.fillText((hard?'Hard Mode: доставки нет, товары только в правом магазине':'Вне магазина дороже: 1–5 = $'+items.map(i=>i.out).join(' / $'))+'   |   ⭐8 справа: $'+starPrice(),450,G+50);
 if(msgT>0){x.font='bold 15px system-ui';x.fillStyle='#000a';x.fillRect(240,366,420,26);x.fillStyle='#fff';x.fillText(msg,450,385)}
 if(over){x.fillStyle='#000d';x.fillRect(0,0,900,420);x.fillStyle='#fff';x.textAlign='center';x.font='bold 36px system-ui';
  x.fillText(over=='win'?'🏁 Ты купил конец игры!':'💀 Игра проиграна',450,190);x.font='16px system-ui';
  x.fillText((over=='win'?(hard?'Hard Mode пройден!'+(newAch.includes('hard')?' Демон разблокирован':''):hw?'Хеллоуинский режим пройден! 🎃':kid?'Детский режим пройден! 🧒':deb?'Режим с дебафами пройден! 🌀':rival?'Магазин конкурента обанкротился! 💥':'Победа!'+(newAch.includes('normal')?' Крипер разблокирован':'')):why)+'  ⏱ '+fmt(tm),450,225);x.fillText(over=='lose'?(!kid?'Возрождения в этом режиме нет — R или «Ресет», чтобы начать сначала':'R — начать заново   |   V / кнопка «Возродиться» — продолжить '+(kid?'бесплатно':'за $500')):'Нажми R, чтобы начать заново',450,255);
  if(over=='win'&&lastTime>0)x.fillText('⏱ '+fmtT(lastTime)+(newRec?'  —  новый рекорд! 🏆':'  —  рекорд: '+fmtT(lastBest)),450,285);
  if(over=='lose'&&msgT>0){x.fillStyle='#ff8080';x.fillText(msg,450,315)}
  }}
(function loop(t){const dt=Math.min(.1,(t-last)/1000);last=t;if(!paused)update(dt);draw();requestAnimationFrame(loop)})(performance.now());
menuRender();
</script><script>
(()=>{
const SUPA_URL='ВСТАВЬТЕ_PROJECT_URL',SUPA_KEY='ВСТАВЬТЕ_ANON_PUBLIC_KEY';
const ok_=!SUPA_URL.startsWith('ВСТАВЬТЕ')&&!SUPA_KEY.startsWith('ВСТАВЬТЕ');
const BASE=SUPA_URL.trim().replace(/\/+$/,'').replace(/\/rest\/v1$/,''),KEY=SUPA_KEY.trim();
const rpc=async(f,b)=>{const h={apikey:KEY,'Content-Type':'application/json'};if(!KEY.startsWith('sb_'))h.Authorization='Bearer '+KEY;
 let r;try{r=await fetch(BASE+'/rest/v1/rpc/'+f,{method:'POST',headers:h,body:JSON.stringify(b)})}catch(e){throw{net:1}}
 if(!r.ok)throw{http:r.status};return r.json()};
const why=e=>e&&e.http?'Ошибка базы (код '+e.http+')'+(e.http==401||e.http==403?': проверьте ключ (anon public или Publishable) и URL':e.http==404||e.http==400?': запустите supabase_setup.sql в Supabase':''):e&&e.net?'Нет связи с базой':'Ошибка. Попробуйте ещё раз.';
const hp=async(u,p)=>[...new Uint8Array(await crypto.subtle.digest('SHA-256',new TextEncoder().encode('shopsim:'+u.toLowerCase()+':'+p)))].map(b=>b.toString(16).padStart(2,'0')).join('');
const loc=()=>{try{return JSON.parse(localStorage.getItem('shoploc'))||{}}catch(e){return{}}};
const lh=v=>typeof v=='string'?{h:v,dn:''}:(v||{});
const saveLoc=L=>{try{localStorage.setItem('shoploc',JSON.stringify(L))}catch(e){}};
let ses=null;try{ses=JSON.parse(localStorage.getItem('shopacc'))}catch(e){}
const saveSes=()=>{try{localStorage.setItem('shopacc',JSON.stringify(ses))}catch(e){}};
const online=()=>ses&&!ses.local&&ok_;
const stats=()=>({ach:[...ach],rec:{...rec},skin:localStorage.getItem('shopskin'),dn:ses&&ses.dn,adm:+localStorage.getItem('shopadm')||0});
function merge(s){let ch=false;s=s||{};
 if(s.adm>(+localStorage.getItem('shopadm')||0)){ach.clear();(s.ach||[]).forEach(id=>{if(ACHS.some(a=>a.id==id))ach.add(id)});
  for(const m in rec)rec[m]=(s.rec&&typeof s.rec[m]=='number'&&s.rec[m]>0)?s.rec[m]:0;
  try{localStorage.setItem('shopadm',s.adm);ach.has('normal')?localStorage.setItem('shopwon','1'):localStorage.removeItem('shopwon');ach.has('hard')?localStorage.setItem('shophard','1'):localStorage.removeItem('shophard')}catch(e){}ch=true}
 (s.ach||[]).forEach(id=>{if(!ach.has(id)&&ACHS.some(a=>a.id==id)){ach.add(id);ch=true}});
 for(const m in rec){const v=s.rec&&s.rec[m];if(typeof v=='number'&&v>0&&(!(rec[m]>0)||v<rec[m])){rec[m]=v;ch=true}}
 if(s.skin&&!localStorage.getItem('shopskin')){try{localStorage.setItem('shopskin',s.skin)}catch(e){}ch=true}
 if(ch){try{saveAch();localStorage.setItem('shoprec',JSON.stringify(rec))}catch(e){}}return ch}
let tm_;
const push=()=>{if(!ses)return;if(ses.local){const L=loc();if(L[ses.u]){L[ses.u]={...lh(L[ses.u]),st:stats()};saveLoc(L)}return}
 if(!ok_)return;clearTimeout(tm_);tm_=setTimeout(()=>rpc('acc_save',{u:ses.u,p:ses.h,s:stats()}).catch(()=>{}),800)};
const flush=async()=>{if(!ses)return;if(ses.local){push();return}if(ok_)try{await rpc('acc_save',{u:ses.u,p:ses.h,s:stats()})}catch(e){}};
const reset=()=>{ses=null;clearTimeout(tm_);['shopach','shoprec','shopskin','shopwon','shophard','shopadm','shopacc'].forEach(k=>{try{localStorage.removeItem(k)}catch(e){}});location.reload()};
const _sa=window.saveAch,_rw=window.recWin,_mr=window.menuRender;
window.saveAch=function(){_sa();push()};window.recWin=function(){_rw();push()};window.menuRender=function(){_mr();push()};
document.addEventListener('visibilitychange',()=>{if(document.hidden)push()});
if(online())rpc('acc_login',{u:ses.u,p:ses.h}).then(r=>{
 if(r.ok){const sd=r.stats&&r.stats.dn;if(typeof sd=='string'&&sd.toLowerCase()==ses.u)ses.dn=sd;ses.id=r.id;ses.admin=!!r.admin;saveSes();updateNick();if(merge(r.stats))location.reload();else push()}
 else if(r.err=='auth')reset()}).catch(()=>{});
const E=(t,p,c)=>{const e=document.createElement(t);Object.assign(e,p||{});(c||[]).forEach(x=>e.append(x));return e};
const ov=E('div');ov.style.cssText='display:none;position:fixed;inset:0;z-index:9999;background:rgba(0,0,0,.6);align-items:center;justify-content:center;padding:12px';
const pn=E('div');pn.style.cssText='background:var(--bg);color:var(--fg);border-radius:14px;padding:18px;max-width:380px;width:100%;max-height:90vh;overflow:auto;text-align:left';
ov.append(pn);document.body.append(ov);ov.onclick=e=>{if(e.target===ov)ov.style.display='none'};
const fT=t=>{const m=Math.floor(t/60),q=(t%60).toFixed(2);return m+':'+(q.length<5?'0':'')+q};
const inp=(ph,ty)=>{const i=E('input',{placeholder:ph,type:ty||'text'});i.style.cssText='width:100%;box-sizing:border-box;padding:9px;margin:4px 0;font-size:16px;border-radius:8px;border:1px solid #888';return i};
const NAMEOK=/^[a-z0-9_а-яё]{3,16}$/i;
const boxS=(bg,fg)=>'background:'+bg+';color:'+fg+';border-radius:8px;padding:8px;font-size:13px;margin:8px 0';
const errM=()=>{const m=E('div');m.style.cssText='min-height:20px;margin:6px 0;color:#d33';return m};
const ping=async()=>{if(!ok_)return '❌ База не подключена: вставьте URL и ключ в index.html (SUPA_URL, SUPA_KEY).';
 try{await rpc('acc_search',{q:'zzzzzzzz'});return '🌐 Подключено к базе сайта.'}catch(e){return '❌ '+(e&&e.http?why(e):'Нет связи с базой, работает локальный режим.')}};
function render(){updateNick();pn.textContent='';const msg=errM();
 pn.append(E('h3',{textContent:'👤 Аккаунт'}));
 const w=E('div',{textContent:'⚠️ Создавайте аккаунт именно на сайте игры. Только такой аккаунт виден всем и работает на любом устройстве. Аккаунт, созданный вне сайта, хранится лишь на этом устройстве и найти его никто не сможет.'});
 w.style.cssText=boxS('#fff3cd','#664d03');
 const st=E('div',{textContent:'Проверка связи…'});st.style.cssText='font-size:12px;margin:4px 0';pn.append(w,st);ping().then(t=>st.textContent=t);
 if(ses){pn.append(E('div',{textContent:'Вы вошли как: '+(ses.dn||ses.u)}),E('div',{textContent:ses.local?'📴 Локальный аккаунт · ID: 0 (только это устройство)':'🌐 Аккаунт на сайте · ID: '+(ses.id||'?')+(ses.admin?' · 🛡️ Администратор':'')}),E('br'),
  E('button',{textContent:'✏️ Редактировать аккаунт',onclick:renderEdit}),' ',
  E('button',{textContent:'Выйти',onclick:async()=>{await flush();reset()}}));
  const n=E('div',{textContent:'При выходе прогресс на этом устройстве сбрасывается (он сохранён в аккаунте), а иконка становится обычной.'});n.style.cssText='font-size:12px;opacity:.75;margin-top:6px';pn.append(n)}
 else{const u=inp('Ник (3–16: буквы, цифры, _)'),p=inp('Пароль (от 6 символов)','password');
  const go=async reg=>{msg.style.color='#d33';const n=u.value.trim(),nl=n.toLowerCase();
   if(!NAMEOK.test(n)||p.value.length<6){msg.textContent='Ник: 3–16 символов (буквы, цифры, _), пароль от 6.';return}
   try{const h=await hp(n,p.value);let on=ok_,r;
    if(ok_){try{if(reg){r=await rpc('acc_register',{u:nl,p:h});if(r.err){msg.textContent=r.err=='taken'?'Ник занят.':r.err=='reserved'?'Этот ник зарезервирован.':'Недопустимый ник.';return}}
     r=await rpc('acc_login',{u:nl,p:h})}catch(e){if(e&&e.net)on=false;else throw e}}else on=false;
    if(on){if(!r.ok){msg.textContent='Неверный ник или пароль.';return}
     const sd=r.stats&&r.stats.dn;ses={u:nl,h,dn:(typeof sd=='string'&&sd.toLowerCase()==nl)?sd:n,id:r.id,admin:!!r.admin};saveSes();
     const ch=merge(r.stats);await rpc('acc_save',{u:nl,p:h,s:stats()});if(ch)location.reload();else render()}
    else{const L=loc();
     if(reg){if(L[nl]){msg.textContent='Такой локальный ник уже есть.';return}L[nl]={h,dn:n,t:Date.now()};saveLoc(L)}
     else if(lh(L[nl]).h!==h){msg.textContent='Неверный ник или пароль. Связи с сайтом нет, проверен только локальный аккаунт.';return}
     ses={u:nl,h,local:true,dn:(reg?n:lh(L[nl]).dn)||n};saveSes();const ch=merge(lh(L[nl]).st);push();
     if(ch){location.reload();return}render();
     const ww=E('div',{textContent:'⚠️ Связи с сайтом нет: создан/открыт ЛОКАЛЬНЫЙ аккаунт, он останется только на этом устройстве. Чтобы аккаунт был виден всем, создайте его на сайте игры.'});
     ww.style.cssText='color:#d33;font-size:13px;margin-top:8px';pn.append(ww)}
   }catch(e){msg.textContent=why(e)}};
  pn.append(u,p,E('div',{},[E('button',{textContent:'Войти',onclick:()=>go(false)}),' ',E('button',{textContent:'Создать аккаунт',onclick:()=>go(true)})]))}
 pn.append(msg,E('div',{},[E('br'),E('button',{textContent:'Закрыть',onclick:()=>ov.style.display='none'})]))}
function renderDel(){pn.textContent='';pn.append(E('h3',{textContent:'🗑️ Удаление аккаунта'}));
 const w=E('div',{textContent:'⚠️ Внимание! Аккаунт «'+(ses.dn||ses.u)+'» будет удалён навсегда'+(ses.local?'':' с сайта')+': ник, ID и сохранённые в аккаунте достижения и рекорды пропадут, восстановить их будет нельзя. Ник освободится, и его сможет занять другой игрок. Прогресс на этом устройстве тоже будет сброшен.'});
 w.style.cssText=boxS('#f8d7da','#58151c');
 const cp=inp('Введите пароль для подтверждения','password'),msg=errM();
 const del=async()=>{try{if(await hp(ses.u,cp.value)!==ses.h){msg.textContent='Неверный пароль.';return}
   if(ses.local){const L=loc();delete L[ses.u];saveLoc(L)}
   else{const r=await rpc('acc_delete',{u:ses.u,p:ses.h});if(!r.ok){msg.textContent=r.err=='admin'?'Аккаунт администратора удалить нельзя.':'Не удалось удалить аккаунт.';return}}
   reset()}catch(e){msg.textContent=why(e)}};
 pn.append(w,cp,msg,E('button',{textContent:'Да, удалить навсегда',onclick:del}),' ',E('button',{textContent:'Отмена',onclick:renderEdit}))}
function renderEdit(){pn.textContent='';pn.append(E('h3',{textContent:'✏️ Редактировать аккаунт'}));
 const nn=inp('Новый ник'),cp=inp('Текущий пароль (обязательно)','password'),np=inp('Новый пароль (если нужно сменить)','password');
 nn.value=ses.dn||ses.u;const msg=errM();
 const save=async()=>{const n=nn.value.trim(),nl=n.toLowerCase();
  if(!NAMEOK.test(n)){msg.textContent='Ник: 3–16 символов (буквы, цифры, _).';return}
  if(np.value&&np.value.length<6){msg.textContent='Новый пароль от 6 символов.';return}
  try{if(await hp(ses.u,cp.value)!==ses.h){msg.textContent='Неверный текущий пароль.';return}
   const nh=await hp(nl,np.value||cp.value);
   if(ses.local){const L=loc();if(nl!==ses.u&&L[nl]){msg.textContent='Такой ник уже занят.';return}
    const o=lh(L[ses.u]);delete L[ses.u];L[nl]={...o,h:nh,dn:n};saveLoc(L)}
   else{const r=await rpc('acc_update',{u:ses.u,p:ses.h,nu:nl,np:nh});
    if(r.err){msg.textContent=r.err=='taken'?'Такой ник уже занят.':r.err=='admin'?'Администратору нельзя менять ник.':r.err=='auth'?'Ошибка входа.':'Недопустимый ник.';return}}
   ses={...ses,u:nl,h:nh,dn:n};saveSes();
   if(ses.local)push();else await rpc('acc_save',{u:ses.u,p:ses.h,s:stats()}).catch(()=>{});
   render();const ok=E('div',{textContent:'✅ Сохранено'});ok.style.color='#2a9d3f';pn.append(ok)
  }catch(e){msg.textContent=why(e)}};
 pn.append(nn,cp,np,msg,E('button',{textContent:'Сохранить',onclick:save}),' ',E('button',{textContent:'Назад',onclick:render}),E('hr'),Object.assign(E('button',{textContent:'🗑️ Удалить аккаунт',onclick:renderDel}),{style:'color:#d33'}))}
const nk=E('div');nk.style.cssText='font-weight:bold;font-size:17px;margin:2px 0 4px;display:none;word-break:break-all';
document.getElementById('pv').before(nk);
function updateNick(){nk.textContent=ses?(ses.dn||ses.u):'';nk.style.display=ses?'block':'none'}updateNick();
const ov2=E('div');ov2.style.cssText=ov.style.cssText;const pn2=E('div');pn2.style.cssText=pn.style.cssText;ov2.append(pn2);document.body.append(ov2);
ov2.onclick=e=>{if(e.target===ov2)ov2.style.display='none'};
const MODES=[['n','Обычный режим'],['h','Hard Mode'],['w','Хеллоуинский режим'],['k','Детский режим'],['d','Режим с дебафами (3 в минуту)'],['e','Режим с дебафами (10 на всю игру)'],['r','Уничтожь конкурентов']];
let lastQ='';
function iconCv(js,size){let sk=null;try{sk=typeof js=='string'?JSON.parse(js):js}catch(e){}
 if(!(sk&&COLS.includes(sk.c)&&Number.isInteger(sk.e)&&sk.e>=0&&sk.e<EYES.length&&sk.e!=6&&sk.e!=8))sk={c:'#ffffff',e:0,m:true,g:null};
 const r=size>40?4:2,c=document.createElement('canvas');c.width=c.height=32*r;c.style.cssText='width:'+size+'px;height:'+size+'px;flex:none';
 const g=c.getContext('2d');g.scale(r,r);sq(g,2,2,sk.c,sk.e,sk.m!==false,COLS.includes(sk.g)?sk.g:null);return c}
const prs=t=>{t=t.trim();if(!t)return 0;let v;if(t.includes(':')){const a=t.split(':');v=(+a[0])*60+(+a[1].replace(',','.'))}else v=+t.replace(',','.');return isFinite(v)&&v>0&&v<86400?Math.round(v*100)/100:NaN};
function renderP(name,id,s,note,uid){pn2.textContent='';
 const adm=!!(ses&&ses.admin&&!ses.local&&id>0),isA=id==2&&uid=='shopsimulator';
 const top=E('div'),nm=E('div'),b1=E('b',{textContent:name});b1.style.fontSize='18px';nm.append(b1,E('div',{textContent:'ID: '+id+(isA?' · 🛡️ Администратор':'')}));
 top.style.cssText='display:flex;align-items:center;gap:12px;margin-bottom:8px';top.append(iconCv(s.skin,64),nm);pn2.append(top);
 if(note){const n=E('div',{textContent:note});n.style.cssText='font-size:12px;opacity:.75;margin-bottom:6px';pn2.append(n)}
 const box=E('div');box.style.marginTop='10px';let cur=null;
 const bR=E('button',{textContent:'🏁 Рекорды'}),bA=E('button',{textContent:'🏆 Ачивки'}),bM=adm?E('button',{textContent:'🛠 Админ'}):null;
 const hv=()=>new Set(Array.isArray(s.ach)?s.ach:[]);
 const A=(f,x)=>rpc(f,{u:ses.u,p:ses.h,tid:id,...x});
 const sv=async ns=>{const n2={...ns,adm:Date.now()},r=await A('acc_admin_stats',{s:{...s,...n2}});if(r.err)throw r;Object.assign(s,n2)};
 const okM=m=>{m.style.color='#2a9d3f';m.textContent='✅ Сохранено'};
 const fEdit=()=>{box.textContent='';const nn=inp('Ник'),pw=inp('Новый пароль (обязателен при смене ника)','password'),m=errM();nn.value=name;
  box.append(nn,pw,m,E('button',{textContent:'Сохранить',onclick:async()=>{m.style.color='#d33';const n=nn.value.trim(),nl=n.toLowerCase();
   if(!NAMEOK.test(n)){m.textContent='Ник: 3–16 символов.';return}
   if(pw.value&&pw.value.length<6){m.textContent='Пароль от 6 символов.';return}
   if(nl!=uid&&!pw.value){m.textContent='При смене ника нужен новый пароль.';return}
   try{const np=pw.value?await hp(nl,pw.value):'',r=await A('acc_admin_update',{nu:nl,np});
    if(r.err){m.textContent=r.err=='taken'?'Ник занят.':r.err=='admin'?'Нельзя менять ник администратора.':'Ошибка.';return}
    await sv({dn:n});if(uid==ses.u){ses.dn=n;if(np)ses.h=np;saveSes();updateNick()}
    renderP(n,id,s,note,nl)}catch(e){m.textContent=why(e)}}}))};
 const fAch=()=>{box.textContent='';const cb={},h0=hv(),m=errM();
  ACHS.forEach(a=>{const l=E('label'),c=E('input',{type:'checkbox',checked:h0.has(a.id)}),n=E('span',{textContent:a.n});cb[a.id]=c;
   l.style.cssText='display:flex;gap:8px;align-items:center;margin:4px 0';n.style.color=LVC[a.lv];if(a.lv==1)n.style.textShadow='0 0 2px #000';l.append(c,n);box.append(l)});
  box.append(m,E('button',{textContent:'Сохранить',onclick:async()=>{m.style.color='#d33';try{await sv({ach:ACHS.filter(a=>cb[a.id].checked).map(a=>a.id)});okM(m)}catch(e){m.textContent=why(e)}}}))};
 const fRec=()=>{box.textContent='';const f={},m=errM();
  MODES.forEach(([k,l])=>{const d=E('div'),i=inp('м:сс.хх или секунды (пусто — нет рекорда)'),v=(s.rec||{})[k];i.value=typeof v=='number'&&v>0?fT(v):'';f[k]=i;d.append(E('div',{textContent:l}),i);box.append(d)});
  box.append(m,E('button',{textContent:'Сохранить',onclick:async()=>{m.style.color='#d33';const rc={};
   for(const[k,l]of MODES){const v=prs(f[k].value);if(isNaN(v)){m.textContent='Неверное время: '+l;return}rc[k]=v}
   try{await sv({rec:rc});okM(m)}catch(e){m.textContent=why(e)}}}))};
 const fDel=()=>{box.textContent='';const w=E('div',{textContent:'⚠️ Удалить аккаунт «'+name+'» (ID: '+id+') навсегда? Это нельзя отменить.'}),m=errM();w.style.cssText=boxS('#f8d7da','#58151c');
  box.append(w,m,E('button',{textContent:'Да, удалить',onclick:async()=>{try{const r=await A('acc_admin_delete',{});if(r.err){m.textContent=r.err=='admin'?'Нельзя удалить администратора.':'Ошибка.';return}renderS()}catch(e){m.textContent=why(e)}}}),' ',E('button',{textContent:'Отмена',onclick:panel}))};
 const panel=()=>{box.textContent='';const mk=(t,f,c)=>{const b=E('button',{textContent:t,onclick:f});b.style.cssText='margin:3px 3px 3px 0;'+(c||'');return b};
  box.append(mk('✏️ Ник и пароль',fEdit),mk('🏆 Выдать ачивки',fAch),mk('⏱ Время рекордов',fRec),mk('🗑️ Удалить',fDel,'color:#d33'))};
 const show=k=>{cur=cur==k?null:k;box.textContent='';[bR,bA,bM].forEach(b=>{if(b)b.style.fontWeight=''});
  const sb={r:bR,a:bA,m:bM}[cur];if(sb)sb.style.fontWeight='bold';
  if(cur=='r'){const r=s.rec||{};MODES.forEach(([m,l])=>{const d=E('div',{textContent:l+': '+(typeof r[m]=='number'&&r[m]>0?fT(r[m]):'—')});d.style.cssText='font-size:14px;margin:4px 0';box.append(d)})}
  if(cur=='a'){const h0=hv(),L=ACHS.filter(a=>h0.has(a.id));box.append(E('b',{textContent:'Выполнено: '+L.length+' из '+ACHS.length}));
   if(!L.length)box.append(E('div',{textContent:'Пока нет выполненных ачивок.'}));
   L.forEach(a=>{const d=E('div'),t=E('div'),n=E('b',{textContent:a.n}),ds=E('div',{textContent:a.d});d.style.cssText='display:flex;align-items:center;gap:8px;margin:6px 0';
    n.style.color=LVC[a.lv];if(a.lv==1)n.style.textShadow='0 0 2px #000';ds.style.cssText='font-size:12px;opacity:.8';t.append(n,ds);
    d.append(a.face<0?gradCv(28):faceCv(a.face,28),t);box.append(d)})}
  if(cur=='m')panel()};
 bR.onclick=()=>show('r');bA.onclick=()=>show('a');if(bM)bM.onclick=()=>show('m');
 pn2.append(bR,' ',bA,...(bM?[' ',bM]:[]),box,E('div',{},[E('br'),E('button',{textContent:'← К списку',onclick:renderS}),' ',E('button',{textContent:'Закрыть',onclick:()=>ov2.style.display='none'})]))}
function renderS(){pn2.textContent='';pn2.append(E('h3',{textContent:'🔎 Поиск игрока'}));
 const q=inp('Ник игрока (пусто — показать всех)'),info=E('div'),res=E('div');q.value=lastQ;info.style.cssText='font-size:13px;opacity:.8;margin:4px 0';pn2.append(q,info,res);let t,n_=0;
 const card=(name,id,s,me,note,uid)=>{const d=E('div'),h=E('div'),a=E('span',{textContent:name,onclick:()=>renderP(name,id,s,note,uid)});
  d.style.cssText='padding:8px;margin:4px 0;border:1px solid #8886;border-radius:8px';h.style.cssText='display:flex;align-items:center;gap:8px';
  a.style.cssText='font-weight:bold;cursor:pointer;text-decoration:underline';
  h.append(iconCv(s.skin,28),a,E('span',{textContent:'ID: '+id+(me?' · это вы':'')}));d.append(h);
  const r=s.rec||{},nr=Object.values(r).filter(n=>n>0).length;d.append(E('div',{textContent:'🏆 Достижений: '+((s.ach||[]).length)+' · 🏁 Рекордов: '+nr+(r.n>0?' · Обычный: '+fT(r.n):'')}));res.append(d)};
 const run=async()=>{const v=q.value.trim().toLowerCase(),k=++n_;res.textContent='';info.textContent='';
  try{if(!ok_)throw{net:1};const L=await rpc('acc_search',{q:v});if(k!=n_)return;
   info.textContent='🌐 Аккаунты на сайте. Нажмите на ник, чтобы открыть аккаунт. Чем меньше ID, тем выше в списке.';
   if(!L.length)res.textContent='Никого не найдено.';
   L.forEach(x=>{const s=x.s||{};card((typeof s.dn=='string'&&s.dn.toLowerCase()==x.u)?s.dn:x.u,x.i,s,ses&&!ses.local&&ses.u==x.u,'',x.u)})}
  catch(e){if(k!=n_)return;
   if(!(e&&e.net)){info.textContent='❌ '+why(e);return}
   const L=loc(),A=Object.keys(L).filter(u=>!v||u.includes(v)).sort((a,b)=>(lh(L[a]).t||0)-(lh(L[b]).t||0));
   info.textContent='📴 Вы не на сайте: показаны только аккаунты, созданные на этом устройстве (ID: 0).';
   if(!A.length)res.textContent='Локальных аккаунтов нет.';
   A.forEach(u=>{const me=ses&&ses.local&&ses.u==u;card(lh(L[u]).dn||u,0,me?stats():(lh(L[u]).st||{}),me,'Локальный аккаунт.',u)})}};
 q.oninput=()=>{lastQ=q.value;clearTimeout(t);t=setTimeout(run,350)};run();
 pn2.append(E('div',{},[E('br'),E('button',{textContent:'Закрыть',onclick:()=>ov2.style.display='none'})]));setTimeout(()=>q.focus(),50)}
const b=E('button',{textContent:'Войти в аккаунт',onclick:()=>{render();ov.style.display='flex'}});
const alt=document.getElementById('altBtn').parentElement,hd=E('div',{textContent:'Аккаунт'});
hd.style.cssText='border-top:1px solid #8886;margin-top:10px;padding-top:6px;font-size:12px;opacity:.7';
const sbtn=E('button',{textContent:'🔎 Поиск игрока',onclick:()=>{renderS();ov2.style.display='flex'}});
const rw=E('div',{className:'row'},[b]),rw2=E('div',{className:'row'},[sbtn]);alt.after(hd,rw,rw2);
})();
</script>
</body></html>
