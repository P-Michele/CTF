La sessione (PHPSessionID) ha un lock che non permette race condition se non controlliamo il cookie.

Se un server esegue ad un redirect e degli header vengono aggiunti (es: Content-Type) alla prosssima richiesta 
gli header non verranno scartati ma rimarranno

Esiste un 'vulnerabilità' su come vengono trattati i Base64, i caratteri non appartenenti all'alfabeto vengono
scartati solo al momento del decode e non ci sono errori se presenti : https://matan-h.com/one-lfi-bypass-to-rule-them-all-using-base64/

Puoi accedere ad un vettore e alle variabili tramite: https://www.php.net/manual/en/language.variables.variable.php ($$ e ${0})

I backtick permettono di eseguire comandi shell (equivalenti alla funzione shell_exec()).
