<div class="separator" style="clear: both;"><a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhGx2t31gF0AArtaQpu-GamaRxSWtS8uIbdLE0ISmi9Wu8sTzNAx9FadhgFUDdnawxS3aY5eXxiYhrezF-LgpJvFwXUxQumrCIVWJW8rZyq5GSqwkqqXUH2tOvZ7UA6WA-wSAx2WOpQ43ImsVO8gAeHcyS6A2flejBP2MGbJ2JnUwK9R6O0IqXZiNxbr_CJ/s303/photo.png" style="display: block; padding: 1em 0; text-align: center; "><img alt="" border="0" width="320" data-original-height="167" data-original-width="303" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhGx2t31gF0AArtaQpu-GamaRxSWtS8uIbdLE0ISmi9Wu8sTzNAx9FadhgFUDdnawxS3aY5eXxiYhrezF-LgpJvFwXUxQumrCIVWJW8rZyq5GSqwkqqXUH2tOvZ7UA6WA-wSAx2WOpQ43ImsVO8gAeHcyS6A2flejBP2MGbJ2JnUwK9R6O0IqXZiNxbr_CJ/s320/photo.png"/></a></div>
            <!DOCTYPE html>
<html>
<head>
  <title>Video to Audio Converter</title>
  <style>
    body {
      font-family:palatino, sans-serif;
      margin: 0;
      padding: 20px;
      text-align: center;
    }

    h1 {
      margin-bottom: 20px;
    }

    .container {
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    #videoFile {
      margin-bottom: 20px;
    }

    #convertButton {
      padding: 10px 20px;
      font-size: 16px;
      background-color: #FFB300;
      color: black;
      border: none;
      cursor: pointer;
    }

    #downloadLink {
      display: none;
      margin-top: 20px;
      font-size: 16px;
      text-decoration: none;
      background-color: #FFB300;
      color: white;
      padding: 10px 20px;
    }
  </style>
  </script>
</head>
<body>
  <h1>Video to Audio Converter</h1>
  <div class="container">
    <input type="file" id="videoFile" accept="video/*">
    <button id="convertButton" onclick="convertVideoToAudio()">Convert</button>
    <br><br>
    <a id="downloadLink"></a>
  </div>

  <script>
    function convertVideoToAudio() {
      var videoFile = document.getElementById('videoFile').files[0];
      if (videoFile) {
        var reader = new FileReader();
        reader.onload = function (event) {
          var videoData = event.target.result;
          var videoBlob = new Blob([videoData], { type: videoFile.type });
          var videoUrl = URL.createObjectURL(videoBlob);
          var video = document.createElement('video');
          video.src = videoUrl;

          video.onloadedmetadata = function () {
            var audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            var mediaSource = audioCtx.createMediaElementSource(video);
            var audioDestination = audioCtx.createMediaStreamDestination();
            mediaSource.connect(audioDestination);
            var audioStream = audioDestination.stream;

            var mediaRecorder = new MediaRecorder(audioStream);
            var audioChunks = [];

            mediaRecorder.ondataavailable = function (event) {
              audioChunks.push(event.data);
            };

            mediaRecorder.onstop = function () {
              var audioBlob = new Blob(audioChunks, { type: 'audio/wav' });
              var audioUrl = URL.createObjectURL(audioBlob);
              var audio = document.createElement('audio');
              audio.controls = true;
              audio.src = audioUrl;

              var downloadLink = document.getElementById('downloadLink');
              downloadLink.href = audioUrl;
              downloadLink.download = 'audio.wav';
              downloadLink.innerHTML = 'Download Audio';
              downloadLink.style.display = 'block';

              document.body.appendChild(audio);
            };

            mediaRecorder.start();
            video.play();

            setTimeout(function () {
              mediaRecorder.stop();
            }, video.duration * 1000);
          };

          document.body.appendChild(video);
        };

        reader.readAsArrayBuffer(videoFile);
      }
    }
  </script>
</body>
                        </html>
