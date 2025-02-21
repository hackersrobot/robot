
<!DOCTYPE html>
<html lang="es">
<head>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chatbot</title>
    <link rel="stylesheet" href="styles.css">

    
</head>
<body>
    <center>    
        <h1 class="titulo-chat">Chat MR Robot</h1>
        <div id="conciencia"></div>
        <p></p>

        <div id="chatbox"></div>

        <audio id="player" controls>
            <source id="audioSource" />
        </audio>
        <p>
<input type="text" id="userInput" placeholder="Escribe algo..." />   </p>   <table>
   <tr>  
  <td>
 <th> <button onclick="procesarPregunta()"><i class="fas fa-paper-plane"></i> </button></th>
  <th><button onclick="activarMicrofono()"><i class="fas fa-microphone"></i></button></th>
  <th><button onclick="borrarConversacion()"> <i class="fas fa-trash"></i> </button></th>
<th><a href="prueba.html"> <button id="deteccion"><i class="fas fa-camera"></i></button></a></th>
                </td>
   <td><a href="prueba.html"> <button><i class="fas fa-pencil-alt"></i></button> </a></td>      
         
         
         
            </tr>
        </table>
    </center>

    <script>
        const respuestas = {
            "hola": "¡Hola! soy el señor robot ¿Cómo puedo ayudarte?",
            "qué puedes hacer": "Puedo darte información que solicites, hablar contigo y reproducir música",
            "quién eres": "Soy MR ROBOT tu asistente virtual, dime como te puedo ayudar.",
            "gracias": "recuerda estoy para ayudarte, si necesitas algo mas solo dimelo y con gusto te ayudare."
        };

        let historialConversacion = JSON.parse(localStorage.getItem('historialConversacion')) || []; 

        function playMusic(song) {
            let audio = document.getElementById("player");
            let source = document.getElementById("audioSource");

            source.src = song;
            audio.load();
            audio.play();
        }

        function buscarEnWikipedia(consulta) {
            return fetch(`https://es.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(consulta)}`)
                .then(response => response.json())
                .then(data => data.extract || "No encontré información en Wikipedia.")
                .catch(() => "Hubo un error al buscar en Wikipedia.");
        }

        // Función para leer en voz alta
function leerEnVozAlta(texto) {
    // Crear un elemento temporal para quitar etiquetas HTML
    let tempElement = document.createElement("div");
    tempElement.innerHTML = texto;
    
    // Extraer solo el texto sin etiquetas
    let textoPlano = tempElement.textContent || tempElement.innerText || "";

    // Leer solo el texto limpio
    let utterance = new SpeechSynthesisUtterance(textoPlano);
    utterance.lang = "es-ES";
    speechSynthesis.speak(utterance);
}
        
        
        
        
        function obtenerHora() {
            const ahora = new Date();
            const horas = ahora.getHours().toString().padStart(2, '0');
            const minutos = ahora.getMinutes().toString().padStart(2, '0');
            const segundos = ahora.getSeconds().toString().padStart(2, '0');
            return `La hora actual es: ${horas}:${minutos}:${segundos}`;
        }


function mostrarVideoEnYouTube(consulta) {
    let urlBusqueda = `https://www.delenamalan.co.za/search/yandel-150/results?search_query=${encodeURIComponent(consulta)}`;
    
    return `
        Aquí tienes los resultados de tu video: 
        <br>
        <iframe width="260" height="315" 
            src=https://www.youtube.com/results?search_query="=${encodeURIComponent(consulta)}" 
            frameborder="0" allowfullscreen></iframe>
    `;
}

function traducirTexto(texto, idiomaDestino) {
    // Crear URL para Google Translate
    let url = `https://translate.google.com/?sl=auto&tl=${encodeURIComponent(idiomaDestino)}&text=${encodeURIComponent(texto)}&op=translate`;
    
    return `
        Aquí tienes la traducción de "${texto}" al idioma ${idiomaDestino}: 
        <a href="${url}" target="_blank">Ver aquí la traducción</a>
    `;
}





        async function responder(pregunta) {
            pregunta = pregunta.toLowerCase();

// Si la pregunta incluye "quiero ver" o "muéstrame", buscamos imágenes
    if (pregunta.includes("quiero ver") || pregunta.includes("muéstrame")) {
        let descripcion = pregunta.replace("quiero ver", "").replace("muéstrame", "").trim();
        let url = `https://www.google.com/search?hl=es&tbm=isch&q=${encodeURIComponent(descripcion)}`;

        // Retornar el enlace para que el usuario lo haga clic
        return `Aquí tienes el enlace para ver imágenes relacionadas con tu búsqueda: <a href="${url}" target="_blank">${url}</a>`;
    }







if (pregunta.includes("traduce")) {
    let partes = pregunta.split(" ");
    let palabra = partes.slice(1, partes.length - 2).join(" ");
    let idioma = partes[partes.length - 1];

    if (palabra && idioma) {
        return traducirTexto(palabra, idioma);
    } else {
        return "Por favor, especifica qué quieres traducir y a qué idioma.";
    }
}



if (pregunta.includes("video")) {
    let consulta = pregunta.replace("video", "").trim();
    if (consulta) {
        return mostrarVideoEnYouTube(consulta);
    } else {
        return "¿Qué video quieres ver?";
    }
}


            // Respuestas personalizadas con enlaces
     if (pregunta.includes("daima capitulo 17")) {
      return `Aquí tiene el capitulo 17:<iframe src="https://mega.nz/embed/FTlXSDiK#nMT_HwGKrZ4jNoaB905OFI_AG5GAT6-ojFiRxN7y7yk" width="340" height="360" frameborder="0" allowfullscreen></iframe> `;
            }
            
      if (pregunta.includes("daima capitulo 16")) {
      return `a qui tienes el caitulo 16: <iframe src="https://mega.nz/embed/IK8FQQ5B#vWdzInS0Egk4yn_pLdH-hn03pqRCweckwHXIUJWeqXs" width="340" height="360" frameborder="0" allowfullscreen></iframe>`;
            }
            
          if (pregunta.includes("daima capitulo 15")) {
      return `aqui tienes el capitulo 15<iframe src="https://mega.nz/embed/YeFjlKQQ#ofIpOAHqf_ss0yiwhacxVID8xON0MEMwaKpCzvc70Ec" width="340" height="360" frameborder="0" allowfullscreen></iframe>`;
            }
            
            
  if (pregunta.includes("daima capitulo 14")) {
      return `Puedes ver el capitulo 14 aquí, dime puedo hacer algo mas por ti:<iframe src="https://mega.nz/embed/dfMmyQTb#8j3qsn1RBXdQD9DcGcpGmaaln9qcn5Bj1VdV7s-rb58" width="340" height="360" frameborder="0" allowfullscreen></iframe>  `;
            }          
            
            
         if (pregunta.includes("como esta el clima")) {
      return `aquí tienes el clima actual en tu ciudad, dime te puedo ayudar en algo mas: <iframe src=" https://www.clima.com/guatemala/escuintla/escuintla  " width="340" height="360"  target="_blank">MDN Web Docs</iframe>`;
            } 
          
          
          if (pregunta.includes("resultados de la liga española")) {
      return `aqui tienes los resultados de la liga española, dime necesitas algo mas, quieres ver posiciones de la liga española pide verla, estoy para ayudarte:  <iframe src=" https://www.marca.com/resultados/futbol/primera-division.html?intcmp=TABAGEN01&s_kw=resultados  " width="340" height="360"  target="_blank">MDN Web Docs</iframe>`;
            }
          
   if (pregunta.includes("posiciones liga española")) {
      return `aquí tienes las pocisiones de la liga española, dime puedo ayidarte en algo mas: <iframe src=" https://www.marca.com/futbol/primera-division/clasificacion.html?intcmp=TABAGEN01&s_kw=clasificacion  "  width="340" height="360" target="_blank"></iframe>`;
            }
            
            
            
            if (pregunta.includes("champions")) {
      return `a qui estan los partidos de  la champions, esta misma pagina puedes ver posciones y resultados, si deceas algo mas solo dime, estoy para ayudarte: <iframe src="https://www.tudn.com/futbol/uefa-champions-league/resultados "  width="350" height="400" target="_blank"></iframe>`;
            }
            
            
            
            if (pregunta.includes("pantheon")) {
      return `Maddie, una adolescente constantemente acosada, comienza a recibir mensajes de un misterioso extraño que dice ser su padre recientemente fallecido, porque su conciencia fue subida a la nube después de un escáner cerebral experimental. dime puedo ayudarte en algo mas? <a href="https://www.netflix.com/es/title/81937398?s=a&trkid=13747225&trg=cp&vlang=es&clip=81945482 " target="_blank"><img src="panteon.webp" width="100" height="200"></a>`;
            }
            
  if (pregunta.includes("brujo")){
    return `a qui tiene la serie the witcher, una serie donde el brujo se enfrenta a diversas criaturas, la puedes ver en Netflix, as click en la imagen para verla, si hay algo mas con lo que pueda ayudarte solo dimelo: <a href=" https://www.netflix.com/es/title/80189685?s=a&trkid=13747225&trg=cp&vlang=es&clip=81702533" target="_blank"><img src=" brujo.jpg " width="100" height="100" border-radius="50%"></a>`;
  }
   
   
              // Si la pregunta incluye "cultivo de tomate", "cultivo de papa" o "cultivo de lechuga", buscamos en datos.html
 if (pregunta.includes("cultivo de tomate") || pregunta.includes("cultivo de papa") || pregunta.includes("cultivo de lechuga") || pregunta.includes("futbol1") || pregunta.includes("daima18")) {
                let tema = "";
                
                // Determina cuál es el tema específico solicitado
    if (pregunta.includes("cultivo de tomate")) {
   tema = "cultivo-tomate";
               
     } else if (pregunta.includes("cultivo de papa")) {
   tema = "cultivo-papa";
   
    } else if (pregunta.includes("cultivo de lechuga")) {
    tema = "cultivo-lechuga";
    
   } else if (pregunta.includes("futbol1")) { 
     
     tema = "futbol1";
     
  } else if (pregunta.includes("daima18")){
    tema = "daima18";
   } 
   
   // Usar fetch para cargar la página datos.html y buscar el contenido relacionado
  try {
  let response = await fetch('datos.html');
     let html = await response.text();
                    
                    // Crear un div temporal para parsear el contenido HTML
   let parser = new DOMParser();
   let doc = parser.parseFromString(html, 'text/html');

                    // Extraer el contenido específico del tema
   let contenido = doc.getElementById(tema).nextElementSibling.innerHTML;
     return contenido || "No encontré información sobre ese tema.";
       } catch (error) {
      console.error("Error al cargar los datos:", error);
      return "Hubo un error al intentar obtener la información.";
                }
            } 
  

            if (pregunta.includes("hora")) {
                return obtenerHora();
            }

            if (pregunta.includes("reproduce")) {
                let songName = pregunta.replace("reproduce ", "").trim() + ".mp3";
                playMusic(songName);
                return "Reproduciendo " + songName;
            }

            if (respuestas[pregunta]) {
                return respuestas[pregunta];
            }

            return await buscarEnWikipedia(pregunta);
        }

        async function procesarPregunta() {
            let input = document.getElementById("userInput");
            let chatbox = document.getElementById("chatbox");
            let userText = input.value.trim();
            if (userText === "") return;

            historialConversacion.push({ usuario: userText, bot: "" });

            chatbox.innerHTML += `<p><strong>Tú:</strong> ${userText}</p>`;
            input.value = ""; 

            let respuesta = await responder(userText);

            historialConversacion[historialConversacion.length - 1].bot = respuesta;
            chatbox.innerHTML += `<p><strong>Bot:</strong> ${respuesta}</p>`;
            chatbox.scrollTop = chatbox.scrollHeight;

            leerEnVozAlta(respuesta);

            localStorage.setItem('historialConversacion', JSON.stringify(historialConversacion));
        }

        function activarMicrofono() {
            let recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
            recognition.lang = "es-ES";
            recognition.start();

            recognition.onresult = async function(event) {
                let transcripcion = event.results[0][0].transcript;
                document.getElementById("userInput").value = transcripcion;
                await procesarPregunta();
            };
        }

        function borrarConversacion() {
            historialConversacion = [];
            localStorage.removeItem('historialConversacion');
            document.getElementById("chatbox").innerHTML = "";
        }

        window.onload = () => {
            let chatbox = document.getElementById("chatbox");
            historialConversacion.forEach(entry => {
                chatbox.innerHTML += `<p><strong>Tú:</strong> ${entry.usuario}</p>`;
                chatbox.innerHTML += `<p><strong>Bot:</strong> ${entry.bot}</p>`;
            });
            chatbox.scrollTop = chatbox.scrollHeight;
        };
    </script>
</body>
</html>