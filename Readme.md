GPOPEN (Get_prefix and open)

--- English ---

Is an assembly program for Apple // and ProDOS, using MLI calls.
It explains how to open a file when the prefix is not set (enpty).

The ProDOS 8 Technical Reference Manual says at page 53 SET_PREFIX:
When ProDOS is started up, the system prefix is set to the name of the volume in the startup drive.

That not always true, according to my experience : 
When you boot disk containig PRODOS and BASIS.SYSTEM, the prefix is empty.
(Unless you set prefix manually, typing PREFIX /MYVOL at prompt).

The problem in this case is you can't open a file with just its name with a openfile MLI call. If you call openfile, you will get a $40 error.
My guess : ProDOS can't find the file, since it concatenate the prefix (empty in this case) with the filename to get the full filename.


This program is a solution to open a file safely, with just its name, wether the prefix is empty or not.

To do this my program :
- first check if the prefix is empty with get_prefix MLI call.

- If the prefix is empty, it extracts it at memory address $280, and calls set_prefix.
- Then it can calls openfile, then closefile, without error.

- If the prefix is not empty, it can directly open and close a file without any error.

Practically :
Boot gpopen.po disk image with Applewin
Brun my program with -GPOPEN command
--> Prefix is empty, but caught at $280.

Reboot
This time, type PREFIX /BRUNO (name of the volume)
Then, brun my program with -GPOPEN command
Now, the prefix is found by get_prefix MLI call.
And used to openfile.

Same behaviour on my Apple //c.
I uses MERLIN32.exe to compile on PC and Merlin-8 on Apple //

--- French ---

GPOPEN (Get_prefix et open)

Est un programme d'assemblage pour Apple // et ProDOS, utilisant des appels MLI.
Il explique comment ouvrir un fichier lorsque le préfixe n'est pas défini (vide).

Le manuel de référence technique ProDOS 8 indique à la page 53 SET_PREFIX:
Au démarrage de ProDOS, le préfixe système est défini sur le nom du volume dans le lecteur de démarrage.

Ce n'est pas toujours vrai, d'après mon expérience:
Lorsque vous démarrez le disque contenant PRODOS et BASIS.SYSTEM, le préfixe est vide.
(Sauf si vous définissez le préfixe manuellement, en tapant PREFIX /MYVOL à l'invite).

Le problème dans ce cas est que vous ne pouvez pas ouvrir un fichier avec juste son nom avec un appel MLI openfile. Si vous appelez openfile, vous obtiendrez une erreur de $40 .
Je suppose: ProDOS ne peut pas trouver le fichier, car il concatène le préfixe (vide dans ce cas) avec le nom de fichier pour obtenir le nom complet du fichier.


Ce programme est une solution pour ouvrir un fichier en toute sécurité, avec juste son nom, que le préfixe soit vide ou non.

Pour ce faire, mon programme:
- vérifie d'abord si le préfixe est vide avec l'appel de get_prefix MLI.

- Si le préfixe est vide, il l'extrait à l'adresse mémoire $280 et appelle set_prefix.
- Ensuite, il peut appeler openfile, puis closefile, sans erreur.

- Si le préfixe n'est pas vide, il peut directement ouvrir et fermer un fichier sans erreur.

Pratiquement:
Démarrez l'image disque gpopen.po avec Applewin
Lancer mon programme avec la commande -GPOPEN
-> Le préfixe est vide, mais capturé à $280.

Redémarrer
Cette fois, tapez PREFIX /BRUNO (=nom du volume)
Ensuite, lancez mon programme avec la commande -GPOPEN
Maintenant, le préfixe est trouvé par l'appel MLI get_prefix.
Il est utilisé pour ouvrir un fichier.

Même comportement sur mon Apple //c 
J'utilise MERLIN32.exe pour compiler sur PC et Merlin-8 sur Apple //
