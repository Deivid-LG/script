# script

  // Script optimizado para completar automáticamente un formulario de Google Forms sobre AuraUI
// Configurado para respuestas favorables y envío automático
(function() {
    // Función para esperar un tiempo aleatorio
    function esperar(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }
    
    // Función para generar edad aleatoria entre 18 y 28
    function generarEdadAleatoria() {
        return Math.floor(Math.random() * (28 - 18 + 1)) + 18;
    }
    
    // Contador global para seguir el número de envíos
    let contadorEnvios = 0;
    const totalEnvios = 20; // Número total de veces a completar el formulario
    
    // Función para completar y enviar un solo formulario
    async function rellenarFormulario() {
        try {
            console.log(`🔄 Iniciando envío #${contadorEnvios + 1} de ${totalEnvios}...`);
            console.log("🔍 Analizando la estructura del formulario...");
            
            // Encontrar campos de texto
            const camposTexto = document.querySelectorAll('input[type="text"], textarea, input[type="email"]');
            console.log(`Encontrados ${camposTexto.length} campos de texto`);
            
            // 1) Encontrar TODOS los radiogroups en el documento
            const todosLosRadioGroups = document.querySelectorAll('div[role="radiogroup"]');
            console.log(`Encontrados ${todosLosRadioGroups.length} grupos de radio buttons`);
            
            // 2) Encontrar TODAS las listas de checkboxes en el documento
            const todasLasListas = document.querySelectorAll('div[role="list"]');
            console.log(`Encontradas ${todasLasListas.length} listas de checkboxes`);
            
            // Encontrar campo de edad - buscamos el campo que contenga "edad" en su etiqueta o contexto
            let campoEdad = null;
            
            for (let i = 0; i < camposTexto.length; i++) {
                const campo = camposTexto[i];
                // Verificar si el campo o su contenedor tiene etiquetas relacionadas con edad
                const contenedorCampo = campo.closest('.freebirdFormviewerComponentsQuestionBaseRoot');
                const textoContenedor = contenedorCampo ? contenedorCampo.textContent.toLowerCase() : '';
                const labelText = campo.getAttribute('aria-label') || 
                                 campo.getAttribute('placeholder') || 
                                 textoContenedor || '';
                
                if (labelText.includes('edad') || 
                    labelText.includes('años') || 
                    labelText.includes('age')) {
                    campoEdad = campo;
                    break;
                }
            }
            
            // Si no encontramos el campo por label, asumimos que es el tercer campo de texto
            if (!campoEdad && camposTexto.length >= 3) {
                campoEdad = camposTexto[2];
            }
            
            // Completar solo el campo de edad (omitir correo y nombre)
            if (campoEdad) {
                const edad = generarEdadAleatoria().toString();
                
                campoEdad.focus();
                
                for (let i = 0; i < edad.length; i++) {
                    await esperar(20 + Math.random() * 30);
                    campoEdad.value = edad.substring(0, i + 1);
                    
                    const evento = new Event('input', { bubbles: true });
                    campoEdad.dispatchEvent(evento);
                }
                
                campoEdad.blur();
                console.log(`✅ Campo Edad completado: "${edad}"`);
                await esperar(100); // Espera mínima
            }
            
            // Procesar todas las preguntas numeradas que son radiogroups
            console.log("Completando preguntas de opción única (radio buttons)...");
            
            // Para cada radiogroup encontrado (después de las 2 preguntas iniciales + la edad)
            for (let i = 0; i < todosLosRadioGroups.length; i++) {
                const radioGroup = todosLosRadioGroups[i];
                const opciones = Array.from(radioGroup.querySelectorAll('div[role="radio"]'));
                
                if (opciones.length > 0) {
                    // Filtrar cualquier opción que contenga la palabra "otro" o "otra" para preferir opciones predefinidas
                    const opcionesFiltradas = opciones.filter(opcion => {
                        const textoOpcion = opcion.textContent.toLowerCase() || '';
                        return !textoOpcion.includes('otro') && !textoOpcion.includes('otra');
                    });
                    
                    // Si no hay opciones después de filtrar, usar todas las opciones
                    const opcionesFinales = opcionesFiltradas.length > 0 ? opcionesFiltradas : opciones;
                    
                    // Variable para la opción a seleccionar
                    let indice;
                    
                    // MODIFICACIÓN ESPECIAL PARA LA PREGUNTA 2 (primera que automatiza el script)
                    // Recuerda: i=0 es la primera pregunta después de la edad, que sería la pregunta 4 del formulario
                    if (i === 0) { // Pregunta 4 (primera pregunta automatizada)
                        // Preferir opciones 1 y 2
                        const random = Math.random();
                        if (random < 0.5 && opcionesFinales.length >= 1) {
                            // 50% de probabilidad de elegir la opción 1
                            indice = 0; // Índice 0 = primera opción
                        } else if (random < 0.9 && opcionesFinales.length >= 2) {
                            // 40% de probabilidad de elegir la opción 2
                            indice = 1; // Índice 1 = segunda opción
                        } else {
                            // 10% de probabilidad de elegir otra opción aleatoria
                            indice = Math.floor(Math.random() * opcionesFinales.length);
                        }
                        console.log(`✨ Pregunta 4 - Seleccionando opción ${indice + 1} (prioridad en opciones 1 y 2)`);
                    }
                    // MODIFICACIÓN ESPECIAL PARA LA PREGUNTA 3 (segunda que automatiza el script)
                    else if (i === 1) { // Pregunta 5 (segunda pregunta automatizada)
                        // Preferir opciones 1 y 2
                        const random = Math.random();
                        if (random < 0.5 && opcionesFinales.length >= 1) {
                            // 50% de probabilidad de elegir la opción 1
                            indice = 0; // Índice 0 = primera opción
                        } else if (random < 0.9 && opcionesFinales.length >= 2) {
                            // 40% de probabilidad de elegir la opción 2
                            indice = 1; // Índice 1 = segunda opción
                        } else {
                            // 10% de probabilidad de elegir otra opción aleatoria
                            indice = Math.floor(Math.random() * opcionesFinales.length);
                        }
                        console.log(`✨ Pregunta 5 - Seleccionando opción ${indice + 1} (prioridad en opciones 1 y 2)`);
                    }
                    // MODIFICACIÓN ESPECIAL PARA LA PREGUNTA 6 (tercera que automatiza el script)
                    else if (i === 2) { // Pregunta 6 (tercera pregunta automatizada)
                        // Determinar cuántas opciones hay disponibles
                        if (opcionesFinales.length >= 4) {
                            // Si hay al menos 4 opciones, escoger entre la opción 3 y 4 con mayor probabilidad
                            const random = Math.random();
                            if (random < 0.40) {
                                // 40% de probabilidad de elegir la opción 3
                                indice = 2; // Índice 2 = tercera opción (0-indexado)
                            } else if (random < 0.80) {
                                // 40% de probabilidad de elegir la opción 4
                                indice = 3; // Índice 3 = cuarta opción (0-indexado)
                            } else {
                                // 20% de probabilidad de elegir otra opción aleatoria
                                indice = Math.floor(Math.random() * opcionesFinales.length);
                            }
                        } else if (opcionesFinales.length === 3) {
                            // Si solo hay 3 opciones, escoger la tercera con mayor probabilidad
                            const random = Math.random();
                            if (random < 0.70) {
                                indice = 2; // Índice 2 = tercera opción
                            } else {
                                indice = Math.floor(Math.random() * 3);
                            }
                        } else {
                            // Si hay menos de 3 opciones, seleccionar aleatoriamente
                            indice = Math.floor(Math.random() * opcionesFinales.length);
                        }
                        console.log(`✨ Pregunta 6 - Seleccionando opción ${indice + 1} (prioridad en opciones 3 y 4)`);
                    }
                    // Para preguntas de importancia, valoración o precio, seleccionar valores altos (favorable)
                    else if (i === 3) { // Pregunta 7 (cuarta automatizada)
                        // 85% de probabilidad de elegir valores altos (índices mayores)
                        if (Math.random() < 0.85 && opcionesFinales.length > 2) {
                            const tercio = Math.floor(opcionesFinales.length / 3);
                            // Seleccionar entre el tercio superior de las opciones
                            indice = opcionesFinales.length - tercio - Math.floor(Math.random() * tercio);
                        } else {
                            indice = Math.floor(Math.random() * opcionesFinales.length);
                        }
                    }
                    // Para pregunta de disposición a pagar, seleccionar valores medio-altos
                    else if (i === 4 || i === 5) { // Probable pregunta 8-9 (pago)
                        const random = Math.random();
                        if (random < 0.15) {
                            // 15% precio bajo
                            indice = Math.floor(Math.random() * Math.floor(opcionesFinales.length / 3));
                        } else if (random < 0.7) {
                            // 55% precio medio
                            const inicio = Math.floor(opcionesFinales.length / 3);
                            const fin = Math.floor(2 * opcionesFinales.length / 3);
                            indice = inicio + Math.floor(Math.random() * (fin - inicio));
                        } else {
                            // 30% precio alto
                            indice = Math.floor(2 * opcionesFinales.length / 3) + 
                                    Math.floor(Math.random() * Math.ceil(opcionesFinales.length / 3));
                        }
                    }
                    // Para otras preguntas, selección aleatoria completa
                    else {
                        indice = Math.floor(Math.random() * opcionesFinales.length);
                    }
                    
                    // Hacer clic en la opción seleccionada
                    opcionesFinales[indice].click();
                    console.log(`✅ Pregunta ${i + 4} (Radiogroup) completada: seleccionada opción ${indice + 1} de ${opcionesFinales.length}`);
                    
                    // Esperar un tiempo mínimo entre selecciones
                    await esperar(100 + Math.random() * 150);
                }
            }
            
            // Procesar todas las preguntas que son checkboxes (listas)
            console.log("Completando preguntas de opción múltiple (checkboxes)...");
            
            // Para cada lista de checkboxes encontrada
            for (let i = 0; i < todasLasListas.length; i++) {
                const lista = todasLasListas[i];
                const opciones = Array.from(lista.querySelectorAll('div[role="checkbox"]'));
                
                if (opciones.length > 0) {
                    // Filtrar cualquier opción que contenga la palabra "otro" o "otra"
                    const opcionesFiltradas = opciones.filter(opcion => {
                        const textoOpcion = opcion.textContent.toLowerCase() || '';
                        return !textoOpcion.includes('otro') && !textoOpcion.includes('otra');
                    });
                    
                    // Si no hay opciones después de filtrar, usar todas las opciones
                    const opcionesFinales = opcionesFiltradas.length > 0 ? opcionesFiltradas : opciones;
                    
                    // Barajar aleatoriamente las opciones para obtener una selección más variada
                    const opcionesBarajadas = [...opcionesFinales];
                    for (let j = opcionesBarajadas.length - 1; j > 0; j--) {
                        const k = Math.floor(Math.random() * (j + 1));
                        [opcionesBarajadas[j], opcionesBarajadas[k]] = [opcionesBarajadas[k], opcionesBarajadas[j]];
                    }
                    
                    // Seleccionar entre 2 y 5 opciones (más opciones para mostrar mayor interés)
                    const numSeleccionar = 2 + Math.floor(Math.random() * 4);
                    const seleccionadas = [];
                    
                    // Seleccionar opciones
                    for (let j = 0; j < Math.min(numSeleccionar, opcionesBarajadas.length); j++) {
                        opcionesBarajadas[j].click();
                        seleccionadas.push(j + 1);
                        await esperar(100 + Math.random() * 100);
                    }
                    
                    // Calcular el número real de la pregunta (considerando las 3 primeras manuales)
                    const numeroPregunta = i + todosLosRadioGroups.length + 4; // 3 iniciales + radiogroups
                    console.log(`✅ Pregunta checkbox ${numeroPregunta} completada: seleccionadas ${seleccionadas.length} opciones`);
                    await esperar(100);
                }
            }
            
            console.log("✅ Todas las preguntas han sido completadas. Buscando botón de enviar...");
            
            // ENVIAR EL FORMULARIO AUTOMÁTICAMENTE
            // Métodos múltiples para encontrar el botón de envío
            let botonEnviar = null;
            
            // Método 1: Buscar por texto que contiene "Enviar"
            const botonesTexto = Array.from(document.querySelectorAll('button, div[role="button"]')).filter(
                btn => btn.textContent && btn.textContent.trim().toLowerCase().includes('enviar')
            );
            
            if (botonesTexto.length > 0) {
                botonEnviar = botonesTexto[0];
                console.log("Botón de enviar encontrado por texto");
            } 
            
            // Método 2: Buscar botones de navegación en la parte inferior
            if (!botonEnviar) {
                const contenedorNavegacion = document.querySelector('.freebirdFormviewerViewNavigationNavControls');
                if (contenedorNavegacion) {
                    const botonesNav = contenedorNavegacion.querySelectorAll('div[role="button"]');
                    if (botonesNav.length > 0) {
                        // El botón de enviar suele ser el último
                        botonEnviar = botonesNav[botonesNav.length - 1];
                        console.log("Botón de enviar encontrado en controles de navegación");
                    }
                }
            }
            
            // Método 3: Buscar cualquier elemento con apariencia de botón de envío
            if (!botonEnviar) {
                const candidatosBotones = document.querySelectorAll('.freebirdFormviewerViewNavigationButtons div[role="button"]');
                if (candidatosBotones.length > 0) {
                    botonEnviar = candidatosBotones[candidatosBotones.length - 1];
                    console.log("Botón de enviar encontrado por clase CSS");
                }
            }
            
            // Método 4: Último recurso - buscar por atributos comunes en botones de envío
            if (!botonEnviar) {
                const todosLosBotones = document.querySelectorAll('button, div[role="button"]');
                const ultimoBoton = todosLosBotones[todosLosBotones.length - 1];
                
                if (ultimoBoton) {
                    botonEnviar = ultimoBoton;
                    console.log("Usando último botón encontrado como enviar");
                }
            }
            
            // Intentar hacer clic en el botón de enviar
            if (botonEnviar) {
                console.log("✅ Botón de enviar encontrado, enviando formulario...");
                await esperar(500);
                botonEnviar.click();
                console.log("🎉 Formulario enviado con éxito!");
                
                // Incrementar el contador de envíos
                contadorEnvios++;
                console.log(`✅ Envío #${contadorEnvios} completado. Quedan ${totalEnvios - contadorEnvios} envíos.`);
                
                // Si hemos completado todos los envíos, terminar
                if (contadorEnvios >= totalEnvios) {
                    console.log(`🏁 ¡Proceso completado! Se han enviado ${contadorEnvios} formularios.`);
                    return;
                }
                
                // Esperar a que se cargue la página de confirmación
                await esperar(3000);
                
                // Ya que mencionaste que casi no hay opción para ingresar otro, en lugar de buscar "Enviar otra respuesta"
                // Vamos a intentar volver directamente a la URL del formulario
                
                // Extraer la URL base del formulario (sin parámetros)
                const urlBase = window.location.href.split('?')[0];
                console.log(`Intentando volver al formulario: ${urlBase}`);
                
                // Navegar de vuelta al formulario
                window.location.href = urlBase;
                
                // Esperar a que se cargue la página y continuar
                setTimeout(() => {
                    console.log("Página recargada, continuando con el siguiente formulario...");
                    rellenarFormulario();
                }, 3000);
            } else {
                console.error("❌ No se pudo encontrar el botón de enviar. Proceso interrumpido.");
            }
            
        } catch (error) {
            console.error("❌ Error al rellenar el formulario:", error);
            
            // Intentar recuperarse del error
            console.log("Intentando recuperarse del error y continuar con el próximo formulario...");
            await esperar(3000);
            
            // Incrementar contador y continuar
            contadorEnvios++;
            
            if (contadorEnvios >= totalEnvios) {
                console.log(`🏁 Proceso completado con errores. Se intentaron ${contadorEnvios} formularios.`);
                return;
            }
            
            // Intentar recargar la página
            window.location.reload();
            
            // Esperar y continuar
            setTimeout(rellenarFormulario, 3000);
        }
    }
    
    // Iniciar el proceso
    console.log(`🚀 Iniciando script automático para formulario de AuraUI...`);
    console.log(`ℹ️ IMPORTANTE: Completa manualmente los campos de correo y nombre ANTES de ejecutar este script.`);
    console.log(`⏱️ El script comenzará en 3 segundos...`);
    setTimeout(rellenarFormulario, 3000);
})();
