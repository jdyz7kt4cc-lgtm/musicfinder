<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MusicFinder</title>

<style>
    body {
        margin: 0;
        background: #101012;
        color: white;
        font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    .container {
        max-width: 700px;
        margin: auto;
        padding: 40px 20px;
    }

    h1 {
        font-size: 40px;
        margin-bottom: 5px;
    }

    .subtitle {
        color: #999;
        margin-bottom: 30px;
    }

    .card {
        background: #1c1c20;
        border-radius: 22px;
        padding: 25px;
        margin-bottom: 20px;
    }

    button {
        border: none;
        border-radius: 15px;
        padding: 16px 20px;
        font-size: 17px;
        font-weight: 600;
        cursor: pointer;
        background: white;
        color: black;
    }

    .microphone {
        background: #29292e;
        color: white;
        margin-left: 10px;
    }

    #fileInput {
        display: none;
    }

    .track {
        display: flex;
        align-items: center;
        gap: 15px;
        padding: 15px 0;
        border-bottom: 1px solid #303034;
    }

    .track:last-child {
        border-bottom: none;
    }

    .track-icon {
        font-size: 28px;
    }

    .track-name {
        flex: 1;
        word-break: break-word;
    }

    .delete {
        background: #303034;
        color: white;
        padding: 9px 12px;
        font-size: 14px;
    }

    .empty {
        color: #777;
        text-align: center;
        padding: 20px;
    }

    .result {
        text-align: center;
        padding: 25px 0;
    }

    .music-icon {
        font-size: 60px;
        margin-bottom: 15px;
    }

    .status {
        color: #999;
        margin-top: 10px;
    }
</style>
</head>

<body>

<div class="container">

    <h1>MusicFinder</h1>

    <div class="subtitle">
        Твоя личная библиотека музыки
    </div>

    <div class="card">

        <label for="fileInput">
            <button>
                ＋ Добавить музыку
            </button>
        </label>

        <input
            id="fileInput"
            type="file"
            accept="audio/*"
            multiple
        >

        <button
            id="microphoneButton"
            class="microphone"
        >
            🎤 Распознать
        </button>

    </div>


    <div class="card">

        <h2>Результат</h2>

        <div class="result">

            <div class="music-icon">
                🎵
            </div>

            <div id="result">
                Пока ничего не распознано
            </div>

            <div id="status" class="status">
                Добавь музыку в свою библиотеку.
            </div>

        </div>

    </div>


    <div class="card">

        <h2>
            Моя музыка
            <span id="count">(0)</span>
        </h2>

        <div id="library">

            <div class="empty">
                Библиотека пуста
            </div>

        </div>

    </div>

</div>


<script>

let tracks = [];


const fileInput =
    document.getElementById("fileInput");

const library =
    document.getElementById("library");

const count =
    document.getElementById("count");

const result =
    document.getElementById("result");

const status =
    document.getElementById("status");

const microphoneButton =
    document.getElementById("microphoneButton");


/* Добавление музыки */

fileInput.addEventListener("change", function() {

    const files = Array.from(this.files);

    files.forEach(function(file) {

        tracks.push(file);

    });

    showLibrary();

    status.textContent =
        "Добавлено треков: " + tracks.length;

});


/* Отображение библиотеки */

function showLibrary() {

    count.textContent =
        "(" + tracks.length + ")";

    library.innerHTML = "";

    if (tracks.length === 0) {

        library.innerHTML =
            '<div class="empty">Библиотека пуста</div>';

        return;

    }


    tracks.forEach(function(file, index) {

        const track =
            document.createElement("div");

        track.className = "track";


        const icon =
            document.createElement("div");

        icon.className = "track-icon";

        icon.textContent = "🎵";


        const name =
            document.createElement("div");

        name.className = "track-name";

        name.textContent = file.name;


        const deleteButton =
            document.createElement("button");

        deleteButton.className = "delete";

        deleteButton.textContent = "Удалить";


        deleteButton.onclick = function() {

            tracks.splice(index, 1);

            showLibrary();

        };


        track.appendChild(icon);

        track.appendChild(name);

        track.appendChild(deleteButton);

        library.appendChild(track);

    });

}


/* Проверка микрофона */

microphoneButton.onclick =
    async function() {

        if (tracks.length === 0) {

            result.textContent =
                "Сначала добавь музыку";

            status.textContent =
                "В библиотеке пока нет треков.";

            return;

        }


        try {

            const stream =
                await navigator.mediaDevices.getUserMedia({
                    audio: true
                });


            stream
                .getTracks()
                .forEach(function(track) {
                    track.stop();
                });


            result.textContent =
                "Микрофон работает ✓";

            status.textContent =
                "Следующий этап — распознавание музыки.";

        }

        catch(error) {

            result.textContent =
                "Нет доступа к микрофону";

            status.textContent =
                "Разреши доступ к микрофону в Safari.";

        }

    };

</script>

</body>
</html>
