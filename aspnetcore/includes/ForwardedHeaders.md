L’intergiciel (middleware) d’en-têtes transférés doit s’exécuter avant tout autre intergiciel. Cet ordre permet au middleware qui repose sur les informations des en-têtes transférés d’utiliser les valeurs d’en-tête pour le traitement. Pour exécuter l’intergiciel (middleware) d’en-têtes transférés après les diagnostics et l’intergiciel (middleware) de gestion des erreurs, consultez [ordre des intergiciels en-têtes transférés](xref:host-and-deploy/proxy-load-balancer#fhmo).