La macro #. permette RCE se read-eval non è settato a NIL e potrebbe permettere comunque
l'accesso a variabili globali (e non) se si è a conoscenza del package, attraverso: #.app.package::*var*
