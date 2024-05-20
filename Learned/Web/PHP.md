La sessione (PHPSessionID) ha un lock che non permette race condition se non controlliamo il cookie.

Se un server esegue ad un redirect e degli header vengono aggiunti (es: Content-Type) alla prosssima richiesta 
gli header non verranno scartati ma rimarranno
