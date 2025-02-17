# AAPS Omnipod Insulin Pump Driver Documentation

These instructions are for configuring the Omnipod Eros generation pump (**NOT Omnipod Dash**). The Omnipod driver is available as part of AAPS (AAPS) as of version 2.8.

**Ce logiciel est une partie d'un système de pancréas artificiel "Do it yourself" (faire soi-même), et ce n’est pas un produit fini destiné à la mise sur le marché. VOUS devez obligatoirement lire, apprendre et comprendre ce système, y compris la façon de l’utiliser. Attention, vous êtes le seul responsable de ce que vous faite avec ce système.**

```{contents}
contenus::
   :backlinks: entrée
   :depth: 2
```

## Configuration matérielle et logicielle requise

- **Périphérique de communication Pod**

> Component that bridges communication from your AAPS enabled phone to Eros generation pods.
> 
> > - ![OrangeLink](../images/omnipod/OrangeLink.png)  [Site internet OrangeLink](https://getrileylink.org/product/orangelink)
> > - ![RileyLink](../images/omnipod/RileyLink.png) [RileyLink 433MHz](https://getrileylink.org/product/rileylink433)
> > - ![EmaLink](../images/omnipod/EmaLink.png)  [Site internet Emalink](https://github.com/sks01/EmaLink) - [Contact](mailto:getemalink@gmail.com)
> > - ![DiaLink](../images/omnipod/DiaLink.png)  DiaLink - [Contact](mailto:Boshetyn@ukr.net)
> > - ![LoopLink](../images/omnipod/LoopLink.png)  [Site internet LoopLink](https://www.getlooplink.org/) - [Contact](https://jameswedding.substack.com/) - Non testé

- ![Android_phone](../images/omnipod/Android_phone.png)  **Mobile Phone Device**

> Component that will operate AAPS and send control commands to the Pod communication device.
> 
> > - Smartphone compatible du [driver Omnipod Android](https://docs.google.com/spreadsheets/d/1eNtXAWwrdVtDvsvXaR_72wgT9ICjZPNEBq8DbitCv_4/edit) avec une version AAPS 2.8 et [configuration des composants](index-component-setup) associés

- ![Omnipod_Pod](../images/omnipod/Omnipod_Pod.png)  **Insulin Delivery Device**

> Component that will interpret commands received from the Pod communication device originating from your AAPS enable phone.
> 
> > - Un nouveau pod Omnipod (génération Eros - **PAS DASH**)

Ces instructions supposent que vous commencez une nouvelle session pod; si ce n'est pas le cas, soyez patient et essayez de commencer ce processus lors de votre prochain changement de pod.

## Avant de commencer

**SÉCURITÉ D'ABORD** - ne pas essayer ce processus dans un environnement où vous ne pouvez pas récupérer une erreur (pods supplémentaires, insuline, RileyLink chargé et les smartphones sont nécessaires).

**Votre PDM Omnipod ne fonctionnera plus après l'activation de votre pod par le driver AAPS Omnipod**. Vous avez précédemment utilisé votre PDM Omnipod pour envoyer des commandes à votre pod Omnipod Eros. Un pod Omnipod Eros ne permet qu'à un seul appareil de la communiquer avec lui. L'appareil qui active le pod avec succès est le seul appareil autorisé à communiquer avec lui à partir de ce moment. Cela signifie qu'une fois que vous activez un pod Omnipod Eros avec votre RileyLink via le pilote AAPS Omnipod, **vous ne pourrez plus utiliser votre PDM avec votre pod**. Le pilote AAPS Omnipod avec le RileyLink est maintenant votre PDM. *Cela ne veut pas dire que vous pouvez jeter votre PDM, il est recommandé de le garder comme une sauvegarde, et en cas d'urgence si jamais AAPS ne fonctionnait pas correctement.*

**Vous pouvez configurer plusieurs RileyLinks, mais un seul RileyLink à la fois peut communiquer avec un pod.** Le pilote AAPS Omnipod supporte la possibilité d'ajouter plusieurs RileyLinks dans la configuration de RileyLink, cependant, un seul RileyLink à la fois peut être sélectionné pour la communication.

**Votre pod ne s'arrêtera pas lorsque le RileyLink sera hors de portée.** Lorsque votre RileyLink est hors de portée ou que le signal est bloqué pour communiquer avec le pod actif, votre pod continuera à injecter l'insuline basale. Lors de l'activation d'un pod, le profil basal défini dans AAPS sera programmé dans le nouveau pod. Si vous perdez le contact avec le pod, il retournera à ce profil de basal. Vous ne serez pas en mesure d'émettre de nouvelles commandes tant que le RileyLink ne reviendra pas à portée et qu'il n'aura pas à nouveau rétablit la connexion.

**30 min Basal Rate Profiles are NOT supported in AAPS.** If you are new to AAPS and are setting up your basal rate profile for the first time please be aware that basal rates starting on a half hour are not supported and you will need to adjust your basal rate profile to start on the hour. Par exemple, si vous avez un débit de basal de 1,1 unités qui commence à 09h30 et a une durée de 2 heures se terminant à 11h30, cela ne marchera pas.  Vous devrez mettre à jour ce taux de basal de 1,1 soit sur une plage horaire de 9h00 à 11h00 soit de 10h00 à 12h00.  Even though the 30 min basal rate profile increments are supported by the Omnipod hardware itself, AAPS is not able to take them into account with its algorithms currently.

## Activation du pilote Omnipod dans AAPS

Vous pouvez activer le pilote Omnipod dans AAPS de **deux façons**:

### Option 1 : L'Assistant de configuration

After installing a new version of AAPS, the **Setup Wizard** will start automatically.  Cela se produit également lors des mises à jour.  Si vous avez déjà exporté vos paramètres à partir d'une installation précédente, vous pouvez quitter l'assistant d'installation et importer vos anciens paramètres.  Pour les nouvelles installations, procédez comme suit.

Via l'**Assistant de configuration AAPS (2)** situé dans le coin supérieur droit **menu trois points (1)**, passez par les menus de l'assistant jusqu'à ce que vous arriviez à l'écran **Pompe**. Ensuite, sélectionnez le **bouton radio Omnipod (3)**.

> ![Enable_Omnipod_Driver_1](../images/omnipod/Enable_Omnipod_Driver_1.png)  ![Enable_Omnipod_Driver_2](../images/omnipod/Enable_Omnipod_Driver_2.png)

Sur le même écran, sous la sélection de la pompe, les **Paramètres du pilote Omnipod** s'affichent, dans la **Configuration du RileyLink** ajoutez votre appareil RileyLink en appuyant sur le texte **Non configuré**.

Sur l'écran de **Recherche du RileyLink** appuyez sur le bouton **Rechercher** et sélectionnez votre RileyLink en scannant tous les périphériques Bluetooth disponibles et en sélectionnant votre RileyLink dans la liste. Lorsque c'est correctement sélectionné, vous êtes basculé sur l'écran de sélection de la pompe, qui affiche les paramètres du pilote Omnipod montrant votre RileyLink sélectionné avec l'adresse MAC listée.

Appuyez sur le bouton **Suivant** pour continuer avec le reste de l'**Assistant de configuration**. Cela peut prendre jusqu'à une minute pour que le RileyLink sélectionné s'initialise et que le bouton **Suivant** devienne actif.

Les étapes détaillées sur la façon de configurer votre appareil de communication pod sont listées ci-dessous dans la section [Configuration RileyLink](OmnipodEros-rileylink-setup).

**OU**

### Option 2 : Le Générateur de configuration

Via le **menu hamburger** situé dans le coin supérieur gauche, dans la **Configuration (1)** ➜**Pompe**➜**Omnipod** en sélectionnant le **bouton radio (2) Omnipod**. En sélectionnant la **case à cocher (4)** à côté de la **roue crantée (3)** cela affichera le menu Omnipod sous la forme d'un onglet intitulé **POD** dans l'interface AAPS. C'est ce que l'on appelle dans cette documentation l'onglet **Omnipod (POD)**.

> **REMARQUE :** Un moyen plus rapide d'accéder aux **paramètres Omnipod** est décrit ci-dessous dans la section [Paramètres Omnipod](OmnipodEros-omnipod-settings) de ce document.
> 
> ![Enable_Omnipod_Driver_3](../images/omnipod/Enable_Omnipod_Driver_3.png) ![Enable_Omnipod_Driver_4](../images/omnipod/Enable_Omnipod_Driver_4.png)

### Vérification de la sélection du pilote Omnipod

*Remarque : Si vous avez quitté l'Assistant de configuration plus tôt sans sélectionner votre RileyLink, Le pilote Omnipod est activé mais vous devrez toujours sélectionner votre RileyLink.  Vous pouvez voir l'onglet Omnipod (POD) s'afficher comme ci-dessous*

Pour vérifier que vous avez activé le pilote Omnipod dans AAPS **glissez vers la gauche** depuis l'onglet **Aperçu**, où vous verrez maintenant un onglet **Omnipod** ou **POD**.

![Enable_Omnipod_Driver_5](../images/omnipod/Enable_Omnipod_Driver_5.png)

## Configuration Omnipod

Veuillez **glisser vers la gauche** jusqu'à l'onglet **Omnipod (POD)** où vous pourrez gérer toutes les fonctions du pod et du RileyLink (certaines de ces fonctions ne sont pas activées ou visibles sans une session de pod active):

> ![refresh_pod_status](../images/omnipod/ICONS/omnipod_overview_refresh_pod_status.png) Refresh Pod connectivity and status
> 
> ![pod_management](../images/omnipod/ICONS/omnipod_overview_pod_management.png) Gestion du Pod (activer, désactiver, tester les bips, stats du RileyLink et historique du Pod)

(OmnipodEros-rileylink-setup)=

### Configuration RileyLink

Si vous avez déjà appairé avec succès votre RileyLink dans l'assistant de configuration ou les étapes ci-dessus, alors procédez à [l'activation d'un Pod](OmnipodEros-activating-a-pod) ci-dessous.

*Remarque : Un bon indicateur visuel indiquant que le RileyLink n'est pas connecté est que les boutons Insuline et Assistant de l'onglet ACCUEIL seront manquants. Cela se produira également pendant environ les 30 premières secondes après le démarrage d'AAPS, car il se connecte activement au RileyLink.*

1. Assurez-vous que votre RileyLink est complètement chargé et mis en marche.

2. Après avoir sélectionné le pilote Omnipod, identifiez et sélectionnez votre RileyLink dans la **Configuration (1)** ➜**Pompe**➜**Omnipod**➜**Roue crantée (2)** ➜**Configuration du RileyLink (3)** en appuyant sur **Non configuré** ou sur le texte de **l'adresse MAC (si présent)**.

   > Assurez-vous que votre batterie RileyLink est chargée et qu'il est [positionné à proximité](OmnipodEros-optimal-omnipod-and-rileylink-positioning) (~30 cm de distance ou moins) de votre téléphone pour que AAPS l'identifie par son adresse MAC. Une fois sélectionné, vous pouvez continuer à activer votre première session de pod. Utilisez le bouton retour de votre téléphone pour revenir à l'interface principale AAPS.
   > 
   > ![RileyLink_Setup_1](../images/omnipod/RileyLink_Setup_1.png) ![RileyLink_Setup_2](../images/omnipod/RileyLink_Setup_2.png)

3. Sur l'écran de **Sélection RileyLink** appuyez sur le bouton **Rechercher (4)** pour lancer un balayage bluetooth. **Sélectionnez votre RileyLink (5)** dans la liste des périphériques Bluetooth disponibles.

   > ![RileyLink_Setup_3](../images/omnipod/RileyLink_Setup_3.png) ![RileyLink_Setup_4](../images/omnipod/RileyLink_Setup_4.png)

4. Après avoir sélectionné avec succès vous êtes revenu sur la page des paramètres Omnipod indiquant l'**adresse MAC du RileyLink actuellement sélectionné (6).**

   > ![RileyLink_Setup_5](../images/omnipod/RileyLink_Setup_5.png)

5. Vérifiez que dans l'onglet **Omnipod (POD)** que l'**Etat du RileyLink (1)** apparaît comme étant **connecté.** Le champ **Etat du pod (2)** doit afficher **Aucun Pod actif**; si ce n'est pas le cas, veuillez réessayer l'étape précédente ou quitter AAPS pour voir si cela rafraîchit la connexion.

   > ![RileyLink_Setup_6](../images/omnipod/RileyLink_Setup_6.png)

(OmnipodEros-activating-a-pod)=

### Activation d’un Pod

Avant de pouvoir activer un pod, veuillez vous assurer que vous avez correctement configuré et connecté votre connexion RileyLink dans les paramètres d'Omnipod

*RAPPEL : La communication avec le Pod pour l'activation de celui-ci se fait sur des périodes limitées pour des raisons de sécurité. Avant d'être appairé le signal radio du Pod est plus faible, mais après l'appairage, il fonctionnera à pleine puissance. Lors de ces procédures, assurez-vous que votre pod est* [à proximité immédiate](OmnipodEros-optimal-omnipod-and-rileylink-positioning) (~30 cm de distance ou moins) mais pas au dessus ou juste à côté du RileyLink.\*

01. Naviguez vers l'onglet **Omnipod (POD)** et cliquez sur le bouton **POD MGMT (1)** , puis cliquez sur **Activer Pod (2)**.

    > ![Activate_Pod_1](../images/omnipod/Activate_Pod_1.png) ![Activate_Pod_2](../images/omnipod/Activate_Pod_2.png)

02. L'écran **Remplir le pod** s'affiche. Remplissez le nouveau pod avec au moins 80 unités d'insuline et écoutez le deux bips indiquant que le pod est prêt à être amorcé. Lors du calcul de la quantité totale d'insuline dont vous avez besoin pendant 3 jours, veuillez prendre en compte que l'amorçage du pod utilisera de 12 à 15 unités.

    > ![Activate_Pod_3](../images/omnipod/Activate_Pod_3.png)
    > 
    > Assurez-vous que le nouveau pod et le RileyLink sont à proximité les uns des autres (~30 cm ou moins) et cliquez sur le bouton **Suivant**.

03. Sur l'écran **Initialiser le Pod** le pod commencera à amorcer (vous entendrez un clic suivi d'une série de tics pendant que le pod s'amorce). Si le RileyLink est hors de portée du pod en cours d'activation, vous recevrez un message d'erreur **Aucune réponse du Pod**. Si cela se produit, [rapprochez le RileyLink](OmnipodEros-optimal-omnipod-and-rileylink-positioning) (~30 cm de distance ou moins) mais pas au dessus ni juste à côté du Pod et cliquez sur le bouton **Réessayer (1)**.

    > ![Activate_Pod_4](../images/omnipod/Activate_Pod_4.png) ![Activate_Pod_5](../images/omnipod/Activate_Pod_5.png)

04. Une fois amorcé avec succès, une coche verte sera affichée et le bouton **Suivant** sera activé. Cliquer sur le bouton **Suivant** pour terminer l'initialisation de l'amorçage du pod et afficher l'écran **Attacher Pod**.

    > ![Activate_Pod_6](../images/omnipod/Activate_Pod_6.png)

05. Ensuite, préparer le site de perfusion du nouveau pod. Retirez le capuchon en plastique du pod et le papier blanc de l'adhésif et appliquez le pod à l'endroit habituel sur votre corps. Une fois terminé, cliquez sur le bouton **Suivant**.

    > ![Activate_Pod_7](../images/omnipod/Activate_Pod_7.png)

06. La boîte de dialogue **Attacher Pod** apparaîtra. **Ne cliquez sur le bouton Ok QUE si vous êtes prêt à déployer la canule**.

    > ![Activate_Pod_8](../images/omnipod/Activate_Pod_8.png)

07. After pressing **OK**, it may take some time before the Omnipod responds and inserts the cannula (1-2 minutes maximum), so be patient.

    > Si le RileyLink est hors de portée du pod en cours d'activation, vous recevrez un message d'erreur **Aucune réponse du Pod**. If this occurs, move the RileyLink closer (~30 cm away or less) to but not on top of or right next to the Pod and click the **Retry** button.
    > 
    > If the RileyLink is out of Bluetooth range or does not have an active connection to the phone, you will receive an error message **No response from RileyLink**. If this occurs, move the RileyLink closer to the phone and click the **Retry** button.
    > 
    > *REMARQUE : Avant d'insérer la canule, il est recommandé de pincer la peau près du point d'insertion de la canule. Cela permet une insertion en douceur de l'aiguille et réduira les risques d'occlusions.*
    > 
    > ![Activate_Pod_9](../images/omnipod/Activate_Pod_9.png)
    > 
    > ![Activate_Pod_10](../images/omnipod/Activate_Pod_10.png) ![Activate_Pod_11](../images/omnipod/Activate_Pod_11.png)

08. Une coche verte s'affiche, et le bouton **Suivant** est activé en cas d'insertion de canule réussie. Cliquer sur le button **Suivant**.

    > ![Activate_Pod_12](../images/omnipod/Activate_Pod_12.png)

09. L'écran **Pod activé** s'affiche. Cliquer sur le bouton vert **Terminé**. Félicitations ! Vous avez démarré une nouvelle session de Pod actif.

    > ![Activate_Pod_13](../images/omnipod/Activate_Pod_13.png)

10. The **Pod management** menu screen should now display with the **Activate Pod (1)** button *disabled* and the **Deactivate Pod (2)** button *enabled*. Ceci est dû au fait qu'un pod est maintenant actif et que vous ne pouvez pas activer un pod supplémentaire sans désactiver d'abord le pod actuellement actif.

    Click on the back button on your phone to return to the **Omnipod (POD)** tab screen which will now display Pod information for your active pod session, including current basal rate, pod reservoir level, insulin delivered, pod errors and alerts.

    For more details on the information displayed go to the [Omnipod (POD) Tab](OmnipodEros-omnipod-pod-tab) section of this document.

    ![Activate_Pod_14](../images/omnipod/Activate_Pod_14.png) ![Activate_Pod_15](../images/omnipod/Activate_Pod_15.png)

### Deactivating a Pod

Under normal circumstances, the life of a pod should run for three days (72 hours) and an additional 8 hours after the pod expiration warning for a total of 80 hours of pod usage.

Pour désactiver un pod (soit après son expiration soit à cause d'une erreur du pod) :

1. Go to the **Omnipod (POD)** tab, click on the **POD MGMT (1)** button, on the **Pod management** screen click on the **Deactivate Pod (2)** button.

   > ![Deactivate_Pod_1](../images/omnipod/Deactivate_Pod_1.png) ![Deactivate_Pod_2](../images/omnipod/Deactivate_Pod_2.png)

2. On the **Deactivate Pod** screen, first, make sure the RileyLink is in close proximity to the pod but not on top of or right next to the pod, then click on the **Next** button to begin the process of deactivating the pod.

   > ![Deactivate_Pod_3](../images/omnipod/Deactivate_Pod_3.png)

3. The **Deactivating Pod** screen will appear, and you will receive a confirmation beep from the pod that deactivation was successful.

   > ![Deactivate_Pod_4](../images/omnipod/Deactivate_Pod_4.png)
   > 
   > **IF deactivation fails** and you do not receive a confirmation beep, you may receive a **No response from RileyLink** or **No response from Pod message**. Please click on the **Retry (1)** button to attempt deactivation again. If deactivation continues to fail, please click on the **Discard Pod (2)** button to discard the Pod. Vous pouvez maintenant supprimer votre pod car la session active a été désactivée. If your Pod has a screaming alarm, you may need to manually silence it (using a pin or a paperclip) as the **Discard Pod (2)** button will not silence it.
   > 
   > > ![Deactivate_Pod_5](../images/omnipod/Deactivate_Pod_5.png)  ![Deactivate_Pod_6](../images/omnipod/Deactivate_Pod_6.png)

4. Une coche verte apparaîtra une fois la désactivation réussie. Cliquer sur le bouton **Suivant** pour afficher l'écran désactivé du pod. Vous pouvez maintenant supprimer votre pod car la session active a été désactivée.

   > ![Deactivate_Pod_7](../images/omnipod/Deactivate_Pod_7.png)

5. Click on the green button to return to the **Pod management** screen.

   > ![Deactivate_Pod_8](../images/omnipod/Deactivate_Pod_8.png)

6. You are now returned to the **Pod management** menu press the back button on your phone to return to the **Omnipod (POD)** tab. Verify that the **RileyLink Status:** field reports **Connected** and the **Pod status:** field displays a **No active Pod** message.

   > ![Deactivate_Pod_9](../images/omnipod/Deactivate_Pod_9.png)  ![Deactivate_Pod_10](../images/omnipod/Deactivate_Pod_10.png)

### Suspendre et reprendre l'injection d'Insuline

The process below will show you how to suspend and resume insulin pump delivery.

*REMARQUE : si vous ne voyez pas de bouton SUSPENDRE*, son affichage n'a pas été activé dans l'onglet Omnipod (POD). Enable the **Show Suspend Delivery button in Omnipod tab** setting in the [Omnipod settings](OmnipodEros-omnipod-settings) under **Other**.

#### Suspending Insulin Delivery

Use this command to put the active pod into a suspended state. In this suspended state, the pod will no longer deliver any insulin. This command mimics the suspend function that the original Omnipod PDM issues to an active pod.

1. Go to the **Omnipod (POD)** tab and click on the **SUSPEND (1)** button. The suspend command is sent from the RileyLink to the active pod and the **SUSPEND (3)** button will become greyed out. The **Pod status (2)** will display **SUSPEND DELIVERY**.

   > ![Suspend_Insulin_Delivery_1](../images/omnipod/Suspend_Insulin_Delivery_1.png) ![Suspend_Insulin_Delivery_2](../images/omnipod/Suspend_Insulin_Delivery_2.png)

2. When the suspend command is successfully confirmed by the RileyLink a confirmation dialog will display the message **All insulin delivery has been suspended**. Cliquez sur **OK** pour confirmer et continuer.

   > ![Suspend_Insulin_Delivery_3](../images/omnipod/Suspend_Insulin_Delivery_3.png)

3. Your active pod has now suspended all insulin delivery. The **Omnipod (POD)** tab will update the **Pod status (1)** to **Suspended**. The **SUSPEND** button will change to a new **Resume Delivery (2)** button

   > ![Suspend_Insulin_Delivery_4](../images/omnipod/Suspend_Insulin_Delivery_4.png)

#### Reprendre l'injection d'insuline

Utilisez cette commande pour demander au pod actif, actuellement suspendu, de reprendre l'injection d'insuline. Une fois la commande exécutée avec succès, l'insuline sera à nouveau injectée normalement avec le débit de basal défini dans le profil actif pour l'heure actuelle. Le pod acceptera à nouveau les commandes pour les bolus, DBT, et SMB.

1. Go to the **Omnipod (POD)** tab and ensure the **Pod status (1)** field displays **Suspended**, then press the **Resume Delivery (2)** button to start the process to instruct the current pod to resume normal insulin delivery. A message **RESUME DELIVERY** will display in the **Pod status (3)** field, signifying the RileyLink is actively sending the command to the suspended pod.

   > ![Resume_Insulin_Delivery_1](../images/omnipod/Resume_Insulin_Delivery_1.png) ![Resume_Insulin_Delivery_2](../images/omnipod/Resume_Insulin_Delivery_2.png)

2. When the Resume delivery command is successfully confirmed by the RileyLink a confirmation dialog will display the message **Insulin delivery has been resumed**. Cliquez sur **OK** pour confirmer et continuer.

   > ![Resume_Insulin_Delivery_3](../images/omnipod/Resume_Insulin_Delivery_3.png)

3. The **Omnipod (POD)** tab will update the **Pod status (1)** field to display **RUNNING,** and the **Resume Delivery** button will now display the **SUSPEND (2)** button.

   > ![Resume_Insulin_Delivery_4](../images/omnipod/Resume_Insulin_Delivery_4.png)

### Acknowledging Pod Alerts

*NOTE - if you do not see an ACK ALERTS button, it is because it is conditionally displayed on the Omnipod (POD) tab ONLY when the pod expiration or low reservoir alert has been triggered.*

The process below will show you how to acknowledge and dismiss pod beeps that occur when the active pod time reaches the warning time limit before the pod expiration of 72 hours (3 days). This warning time limit is defined in the **Hours before shutdown** Omnipod alerts setting. The maximum life of a pod is 80 hours (3 days 8 hours), however Insulet recommends not exceeding the 72 hour (3 days) limit.

*NOTE - If you have enabled the "Automatically acknowledge Pod alerts" setting in Omnipod Alerts, this alert will be handled automatically after the first occurrence and you will NOT need to manually dismiss the alert.*

1. When the defined **Hours before shutdown** warning time limit is reached, the pod will issue warning beeps to inform you that it is approaching its expiration time and a pod change will soon be required. You can verify this on the **Omnipod (POD)** tab, the **Pod expires: (1)** field will show the exact time the pod will expire (72 hours after activation) and the text will turn **red** after this time has passed, under the **Active Pod alerts (2)** field where the status message **Pod will expire soon** is displayed. This trigger will display the **ACK ALERTS (3)** button. A **system notification (4)** will also inform you of the upcoming pod expiration

   > ![Acknowledge_Alerts_1](../images/omnipod/Acknowledge_Alerts_1.png) ![Acknowledge_Alerts_2](../images/omnipod/Acknowledge_Alerts_2.png)

2. Go to the **Omnipod (POD)** tab and press the **ACK ALERTS (2)** button (acknowledge alerts). The RileyLink sends the command to the pod to deactivate the pod expiration warning beeps and updates the **Pod status (1)** field with **ACKNOWLEDGE ALERTS**.

   > ![Acknowledge_Alerts_3](../images/omnipod/Acknowledge_Alerts_3.png)

3. Upon **successful deactivation** of the alerts, **2 beeps** will be issued by the active pod and a confirmation dialog will display the message **Activate alerts have been acknowledged**. Cliquez sur le bouton **OK** pour confirmer et fermer la boîte de dialogue.

   > ![Acknowledge_Alerts_4](../images/omnipod/Acknowledge_Alerts_4.png)
   > 
   > If the RileyLink is out of range of the pod while the acknowledge alerts command is being processed a warning message will display 2 options. **Mute (1)** will silence this current warning. **OK (2)** will confirm this warning and allow the user to try to acknowledge alerts again.
   > 
   > ![Acknowledge_Alerts_5](../images/omnipod/Acknowledge_Alerts_5.png)

4. Go to the **Omnipod (POD)** tab, under the **Active Pod alerts** field, the warning message is no longer displayed and the active pod will no longer issue pod expiration warning beeps.

(OmnipodEros-view-pod-history)=

### Voir l'historique du Pod

Cette section vous montre comment revoir l'historique du pod actif et filtrer selon les catégories d'action. The pod history tool allows you to view the actions and results committed to your currently active pod during its three day (72 - 80 hours) life.

This feature is useful for verifying boluses, TBRs, basal changes that were given but you may be unsure if they completed. The remaining categories are useful in general for troubleshooting issues and determining the order of events that occurred leading up to a failure.

*NOTE:* **Uncertain** commands will appear in the pod history, however due to their nature you cannot ensure their accuracy.

1. Go to the **Omnipod (POD)** tab and press the **POD MGMT (1)** button to access the **Pod management** menu and then press the **Pod history (2)** button to access the pod history screen.

   > ![Pod_History_1](../images/omnipod/Pod_History_1.png) ![Pod_History_2](../images/omnipod/Pod_History_2.png)

2. On the **Pod history** screen, the default category of **All (1)** is displayed showing the **Date and Time (2)** of all pod **Actions (3)** and **Results (4)** in reverse chronological order. Use your phone’s **back button 2 times** to return to the **Omnipod (POD)** tab in the main AAPS interface.

   > ![Pod_History_3](../images/omnipod/Pod_History_3.png) ![Pod_History_4](../images/omnipod/Pod_History_4.png)

### View RileyLink Settings and History

This section shows you how to review the settings of your active pod and RileyLink along with the communication history of each. This feature, once accessed, is split into two sections: **Settings** and **History**.

The primary use of this feature is when your pod communication device is out of the Bluetooth range of your phone after a period of time and the **RileyLink status** reports **RileyLink unreachable**. The **REFRESH** button on the main **Omnipod (POD)** tab will manually attempt to re-establish Bluetooth communication with the currently configured RileyLink in the Omnipod settings.

In the event the **REFRESH** button on the main **Omnipod (POD)** tab does not restore the connection to the pod communication device, please follow the additional steps below for a manual reconnection.

#### Manually Re-establish Pod Communication Device Bluetooth Communication

1. From the **Omnipod (POD)** tab when the **RileyLink Status: (1)** reports **RileyLink unreachable** press the **POD MGMT (2)** button to navigate to the **Pod Management** menu. On the **Pod Management** menu you will see a notification appear actively searching for a RileyLink connection, press the **RileyLink stats (3)** button to access the **RileyLink settings** screen.

   > ![RileyLink_Bluetooth_Reset_1](../images/omnipod/RileyLink_Bluetooth_Reset_1.png) ![RileyLink_Bluetooth_Reset_2](../images/omnipod/RileyLink_Bluetooth_Reset_2.png)

2. On the **RileyLink Settings (1)** screen under the **RileyLink (2)** section you can confirm both the Bluetooth connection status and error in the **Connection Status and Error: (3)** fields. A *Bluetooth Error* and *RileyLink unreachable* status should be shown. Start the manual Bluetooth reconnection by pressing the **refresh (4)** button in the lower right corner.

   > ![RileyLink_Bluetooth_Reset_3](../images/omnipod/RileyLink_Bluetooth_Reset_3.png)
   > 
   > If the pod communication device is unresponsive or out of range of the phone while the Bluetooth refresh command is being processed a warning message will display 2 options.

   - **Mute (1)** will silence this current warning.
   - **OK (2)** will confirm this warning and allow the user to try to re-establish the Bluetooth connection again.

   > ![RileyLink_Bluetooth_Reset_4](../images/omnipod/RileyLink_Bluetooth_Reset_4.png)

3. If the **Bluetooth connection** does not re-establish, try manually turning **off** and then back **on** the Bluetooth function on your phone.

4. After a successful RileyLink Bluetooth reconnection the **Connection Status: (1)** field should report **RileyLink ready**. Congratulations, you have now reconnected your configured pod communication device to AAPS!

   > ![RileyLink_Bluetooth_Reset_5](../images/omnipod/RileyLink_Bluetooth_Reset_5.png)

#### Pod Communication Device and Active Pod Settings

This screen will provide information, status, and settings configuration information for both the currently configured pod communication device and the currently active Omnipod Eros pod.

1. Go to the **Omnipod (POD)** tab and press the **POD MGMT (1)** button to access the **Pod management** menu, then press the **RileyLink stats (2)** button to view your currently configured **RileyLink (3)** and active pod **Device (4)** settings.

   > ![RileyLink_Statistics_Settings_1](../images/omnipod/RileyLink_Statistics_Settings_1.png) ![RileyLink_Statistics_Settings_2](../images/omnipod/RileyLink_Statistics_Settings_2.png)
   > 
   > ![RileyLink_Statistics_Settings_3](../images/omnipod/RileyLink_Statistics_Settings_3.png)

##### RileyLink (3) fields

> - **Address:** MAC address of the selected pod communication device defined in the Omnipod Settings.
> - **Name:** Bluetooth identification name of the selected pod communication device defined in your phone's Bluetooth settings.
> - **Battery Level:** Displays the current battery level of the connected pod communication device
> - **Connected Device:** Model of the Omnipod pod currently communicating with the pod communication device
> - **Connection Status**: The current status of the Bluetooth connection between the pod communication device and the phone running AAPS.
> - **Connection Error:** If there is an error with the pod communication device Bluetooth connection details will be displayed here.
> - **Firmware Version:** Current firmware version installed on the actively connected pod communication device.

##### Device (4) fields - With an Active Pod

> - **Device Type:** The type of device communicating with the pod communication device (Omnipod pod pump)
> - **Device Model:** The model of the active device connected to the pod communication device (the current model name of the Omnipod pod, which is Eros)
> - **Pump Serial Number:** Serial number of the currently activated pod
> - **Pump Frequency:** Communication radio frequency the pod communication device has tuned to enable communication between itself and the pod.
> - **Last Used frequency:** Last known radio frequency the pod used to communicate with the pod communication device.
> - **Last Device Contact:** Date and time of the last contact the pod made with the pod communication device.
> - **Refresh button** manually refresh the settings on this page.

#### RileyLink and Active Pod History

This screen provides information in reverse chronological order of each state or action that either the RileyLink or currently connected pod is in or has taken. The entire history is only available for the currently active pod, after a pod change this history will be erased and only events from the newly activated pod will be recorded and shown.

1. Go to the **Omnipod (POD)** tab and press the **POD MGMT (1)** button to access the **Pod Management** menu, then press the **Pod History (2)** button to view the **Settings** and **History** screen. Click on the **HISTORY (3)** text to display the entire history of the RileyLink and currently active pod session.

   > ![RileyLink_Statistics_History_1](../images/omnipod/RileyLink_Statistics_History_1.png) ![RileyLink_Statistics_History_2](../images/omnipod/RileyLink_Statistics_History_2.png)
   > 
   > ![RileyLink_Statistics_History_3](../images/omnipod/RileyLink_Statistics_History_3.png)

##### Champs

> - **Date & Time**: In reverse chronological order the timestamp of each event.
> - **Device:** The device to which the current action or state is referring.
> - **State or Action:** The current state or action performed by the device.

(OmnipodEros-omnipod-pod-tab)=

## Omnipod (POD) Tab

Below is an explanation of the layout and meaning of the icons and status fields on the **Omnipod (POD)** tab in the main AAPS interface.

*NOTE: If any message in the Omnipod (POD) tab status fields report (uncertain) then you will need to press the Refresh button to clear it and refresh the pod status.*

> ![Omnipod_Tab](../images/omnipod/Omnipod_Tab.png)

### Champs

- **RileyLink Status:** Displays the current connection status of the RileyLink

- *RileyLink Unreachable* - pod communication device is either not within Bluetooth range of the phone, powered off or has a failure preventing Bluetooth communication.
- *RileyLink Ready* - pod communication device is powered on and actively initializing the Bluetooth connection
- *Connected* - pod communication device is powered on, connected and actively able to communicate via Bluetooth.

- **Pod address:** Displays the current address in which the active pod is referenced

- **LOT:** Displays the LOT number of the active pod

- **TID:** Displays the serial number of the pod.

- **Firmware Version:** Displays the firmware version of the active pod.

- **Time on Pod:** Displays the current time on the active pod.

- **Pod expires:** Displays the date and time when the active pod will expire.

- **Pod status:** Displays the status of the active pod.

- **Last connection:** Displays the last time communication with the active pod was achieved.

- *À l'instant* - il y a moins de 20 secondes.
- *Il y a moins d'une minute* - plus de 20 secondes mais moins de 60 secondes.
- *il y a 1 minute* - plus de 60 secondes mais moins de 120 secondes (2 min)
- *il y a XX minutes* - il y a plus de 2 minutes comme défini par la valeur de XX

- **Last bolus:** Displays the dosage of the last bolus sent to the active pod and how long ago it was issued in parenthesis.

- **Débit de Basal :** Affiche le débit Basal courant en ce moment, à partir du débit de basal du profil.

- **Débit de Basal Temp. :** Affiche le débit de basal Temporaire actuellement en cours d'exécution dans le format suivant

- Units / hour @ time TBR was issued (minutes run / total minutes TBR will be run)
- *Exemple :* 0.00U/h @18:25 (90/120 minutes)

- **Réservoir :** Affiche plus de 50+U quand plus de 50 unités sont dans le réservoir. Below this value the exact units are displayed in yellow text.

- **Total injecté :** Affiche le nombre total d'unités d'insuline injectées depuis le réservoir. *Note this is an approximation as priming and filling the pod is not an exact process.*

- **Erreurs :** Affiche la dernière erreur rencontrée. Review the [Pod history](OmnipodEros-view-pod-history), [RileyLink history](OmnipodEros-rileylink-and-active-pod-history) and log files for past errors and more detailed information.

- **Alertes Pod actif :** Réservées pour les alertes en cours sur le pod actif. Normally used when pod expiration is past 72 hours and native pod beep alerts are running.

### Icons

- **REFRESH:**

  > ![refresh_pod_status](../images/omnipod/ICONS/omnipod_overview_refresh_pod_status.png)
  > 
  > Sends a refresh command to the active pod to update communication
  > 
  > A utiliser pour actualiser l'état du pod et rejeter les champs qui contiennent le texte (incertain).
  > 
  > See the [Troubleshooting section](OmnipodEros-troubleshooting) below for additional information.

- **POD MGMT:**

  > ![pod_management](../images/omnipod/ICONS/omnipod_overview_pod_management.png)
  > 
  > Navigates to the Pod management menu

- **ACK ALERTS:**

  > ![ack_alerts](../images/omnipod/ICONS/omnipod_overview_ack_alerts.png)
  > 
  > When pressed this will disable the pod expiration beeps and notifications.
  > 
  > Button is displayed only when pod time is past expiration warning time Upon successful dismissal, this icon will no longer appear.

- **SET TIME:**

  > ![set_time](../images/omnipod/ICONS/omnipod_overview_set_time.png)
  > 
  > When pressed this will update the time on the pod with the current time on your phone.

- **SUSPEND:**

  > ![suspend](../images/omnipod/ICONS/omnipod_overview_suspend.png)
  > 
  > Suspends the active pod

- **RESUME DELIVERY:**

  > ![resume](../images/omnipod/ICONS/omnipod_overview_resume.png)
  > 
  > > Resumes the currently suspended, active pod

### Menu de Gestion du pod

Below is an explanation of the layout and meaning of the icons on the **Pod Management** menu accessed from the **Omnipod (POD)** tab.

> ![Omnipod_Tab_Pod_Management](../images/omnipod/Omnipod_Tab_Pod_Management.png)

- **Activer le Pod**

  > ![activate_pod](../images/omnipod/ICONS/omnipod_overview_pod_management_activate_pod.png)
  > 
  > Primes and activates a new pod

- **Désactiver Pod**

  > ![deactivate_pod](../images/omnipod/ICONS/omnipod_overview_pod_management_deactivate_pod.png)
  > 
  > Deactivates the currently active pod.
  > 
  > A partially paired pod ignores this command.
  > 
  > Use this command to deactivate a screaming pod (error 49).
  > 
  > If the button is disabled (greyed out) use the Discard Pod button.

- **Play test beep**

  > ![play_test_beep](../images/omnipod/ICONS/omnipod_overview_pod_management_play_test_beep.png)
  > 
  > Plays a single test beep on the pod when pressed.

- **Discard pod**

  > ![discard_pod](../images/omnipod/ICONS/omnipod_overview_pod_management_discard_pod.png)
  > 
  > Deactivates and discards the pod state of an unresponsive pod when pressed.
  > 
  > Button is only displayed when very specific cases are met as proper deactivation is no longer possible:
  > 
  > > - A **pod is not fully paired** and thus ignores deactivate commands.
  > > - A **pod is stuck** during the pairing process between steps
  > > - A **pod simply does not pair at all.**

- **Pod history**

  > ![pod_history](../images/omnipod/ICONS/omnipod_overview_pod_management_pod_history.png)
  > 
  > Displays the active pod activity history

- **RileyLink stats:**

  > ![rileylink_stats](../images/omnipod/ICONS/omnipod_overview_pod_management_rileylink_stats.png)
  > 
  > Navigates to the RileyLink Statistics screen displaying current settings and RileyLink Connection history
  > 
  > > - **Settings** - displays RileyLink and active pod settings information
  > > - **History** - displays RileyLink and Pod communication history

- **Reset RileyLink Config**

  > ![reset_rileylink_config](../images/omnipod/ICONS/omnipod_overview_pod_management_reset_rileylink_config.png)
  > 
  > When pressed this button resets the currently connected pod communication device configuration.
  > 
  > > - When communication is started, specific data is sent to and set in the RileyLink > - Memory Registers are set > - Communication Protocols are set > - Tuned Radio Frequency is set 
  > > - See [additional notes](OmnipodEros-reset-rileylink-config-notes) at the end of this table

- **Read pulse log:**

  > ![pulse_log](../images/omnipod/ICONS/omnipod_overview_pod_management_pulse_log.png)
  > 
  > > Sends the active pod pulse log to the clipboard

(OmnipodEros-reset-rileylink-config-notes)=

#### *Reset RileyLink Config Notes*

- The primary usage of this feature is when the currently active pod communication device is not responding and communication is in a stuck state.
- If the pod communication device is turned off and then back on, the **Reset RileyLink Config** button needs to be pressed, so that it sets these communication parameters in the pod communication device configuration.
- If this is NOT done then AAPS will need to be restarted after the pod communication device is power cycled.
- This button **DOES NOT** need to be pressed when switching between different pod communication devices

(OmnipodEros-omnipod-settings)=

## Omnipod Settings

The Omnipod driver settings are configurable from the top-left hand corner **hamburger menu** under **Config Builder**➜**Pump**➜**Omnipod**➜**Settings Gear (2)** by selecting the **radio button (1)** titled **Omnipod**. Selecting the **checkbox (3)** next to the **Settings Gear (2)** will allow the Omnipod menu to be displayed as a tab in the AAPS interface titled **OMNIPOD** or **POD**. C'est ce que l'on appelle dans cette documentation l'onglet **Omnipod (POD)**.

![Omnipod_Settings_1](../images/omnipod/Omnipod_Settings_1.png)

**NOTE:** A faster way to access the **Omnipod settings** is by accessing the **3 dot menu (1)** in the upper right hand corner of the **Omnipod (POD)** tab and selecting **Omnipod preferences (2)** from the dropdown menu.

![Omnipod_Settings_2](../images/omnipod/Omnipod_Settings_2.png)

Les groupes de paramètres sont listés ci-dessous; vous pouvez les activer ou les désactiver via un commutateur pour la plupart des entrées décrites ci-dessous :

![Omnipod_Settings_3](../images/omnipod/Omnipod_Settings_3.png)

*NOTE: An asterisk (\*) denotes the default for a setting is enabled.*

### RileyLink

Allows for scanning of a pod communication device. The Omnipod driver cannot select more than one pod communication device at a time.

- **Show battery level reported by OrangeLink/EmaLink/DiaLink:** Reports the actual battery level of the OrangeLink/EmaLink/Dialink. It is **strongly recommended** that all OrangeLink/EmaLink/DiaLink users enable this setting.

- DOES NOT work with the original RileyLink.
- May not work with RileyLink alternatives.
- Enabled - Reports the current battery level for supported pod communication devices.
- Disabled - Reports a value of n/a.

- **Enable battery change logging in Actions:** In the Actions menu, the battery change button is enabled IF you have enabled this setting AND the battery reporting setting above.  Some pod communication devices now have the ability to use regular batteries which can be changed.  This option allows you to note that and reset battery age timers.

### Bips de confirmation

Paramètre les bips de confirmation du pod pour l'injection et les modifications de bolus, basal, SMB et DBT.

- **\*Bolus beeps enabled:** Enable or disable confirmation beeps when a bolus is delivered.
- **\*Basal beeps enabled:** Enable or disable confirmation beeps when a new basal rate is set, active basal rate is canceled or current basal rate is changed.
- **\*SMB beeps enabled:** Enable or disable confirmation beeps when a SMB is delivered.
- **Bips DBT activés :** Active ou désactive les bips de confirmation lorsqu'un DBT est défini ou annulé.

### Alertes

Provides AAPS alerts and Nightscout announcements for pod expiration, shutdown, low reservoir based on the defined threshold units.

*Notez qu'une notification AAPS sera TOUJOURS émise pour toute alerte après une première communication avec le pod qui a déclenchée l'alerte. Rejeter la notification ne rejettera PAS l'alerte SAUF SI Accepter automatiquement les alertes Pod est activé. To MANUALLY dismiss the alert you must visit the Omnipod (POD) tab and press the ACK ALERTS button.*

- **\*Expiration reminder enabled:** Enable or disable the pod expiration reminder set to trigger when the defined number of hours before shutdown is reached.
- **Heures avant arrêt :** Définit le nombre d'heures avant l'arrêt du pod actif, ce qui déclenchera ensuite l'alerte de rappel d'expiration.
- **\*Low reservoir alert enabled:** Enable or disable an alert when the pod's remaining units low reservoir limit is reached as defined in the Number of units field.
- **Nombre d'unités :** Défini le nombre d'unités à partir de laquelle l'alerte Réservoir bas est activée.
- **Automatically acknowledge Pod alerts:** When enabled a notification will still be issued however immediately after the first pod communication contact since the alert was issued it will now be automatically acknowledged and the alert will be dismissed.

### Notifications

Provides AAPS notifications and audible phone alerts when it is uncertain if TBR, SMB, or bolus events were successful.

*REMARQUE : Ce ne sont que des notifications, aucune alerte sonores n'est faite.*

- **Son pour les notifications DBT incertains activé :** Activer ou désactiver ce paramètre pour déclencher une alerte audible et une notification visuelle lorsque AAPS n’est pas certain si un DBT a été défini avec succès.
- **\*Sound for uncertain SMB notifications enabled:** Enable or disable this setting to trigger an audible alert and visual notification when AAPS is uncertain if an SMB was successfully delivered.
- **\*Sound for uncertain bolus notifications enabled:** Enable or disable this setting to trigger an audible alert and visual notification when AAPS is uncertain if a bolus was successfully delivered.

### Autres

Provides advanced settings to assist debugging.

- **Show Suspend Delivery button in Omnipod tab:** Hide or display the suspend delivery button in the **Omnipod (POD)** tab.
- **Show Pulse log button in Pod Management menu:** Hide or display the pulse log button in the **Pod Management** menu.
- **Show RileyLink Stats button in Pod Management menu:** Hide or display the RileyLink Stats button in the **Pod Management** menu.
- **\*DST/Time zone detect on enabled:** allows for time zone changes to be automatically detected if the phone is used in an area where DST is observed.

### Switching or Removing an Active Pod Communication Device (RileyLink)

With many alternative models to the original RileyLink available (such as OrangeLink or EmaLink) or the need to have multiple/backup versions of the same pod communication device (RileyLink), it becomes necessary to switch or remove the selected pod communication device (RileyLink) from Omnipod Setting configuration.

The following steps will show how to **Remove** and existing pod communication device (RileyLink) as well as **Add** a new pod communication device.  Executing both **Remove** and **Add** steps will switch your device.

1. Access the **RileyLink Selection** menu by selecting the **3 dot menu (1)** in the upper right hand corner of the **Omnipod (POD)** tab and selecting **Omnipod preferences (2)** from the dropdown menu. On the **Omnipod Settings** menu under **RileyLink Configuration (3)** press the **Not Set** (if no device is selected) or **MAC Address** (if a device is present) text to open the **RileyLink Selection** menu.

   > ![Omnipod_Settings_2](../images/omnipod/Omnipod_Settings_2.png) ![RileyLink_Setup_2](../images/omnipod/RileyLink_Setup_2.png)

### Remove Currently Selected Pod Communication Device (RileyLink)

This process will show how to remove the currently selected pod communication device (RileyLink) from the Omnipod Driver settings.

1. Under **RileyLink Configuration** press the **MAC Address (1)** text to open the **RileyLink Selection** menu.

   > ![RileyLink_Setup_Remove_1](../images/omnipod/RileyLink_Setup_Remove_1.png)

2. On the **RileyLink Selection** menu the press **Remove (2)** button to remove **your currently selected RileyLink (3)**

   > ![RileyLink_Setup_Remove_2](../images/omnipod/RileyLink_Setup_Remove_2.png)

3. At the confirmation prompt press **Yes (4)** to confirm the removal of your device.

   > ![RileyLink_Setup_Remove_3](../images/omnipod/RileyLink_Setup_Remove_3.png)

4. You are returned to the **Omnipod Setting** menu where under **RileyLink Configuration** you will now see the device is **Not Set (5)**.  Congratulations, you have now successfully removed your selected pod communication device.

   > ![RileyLink_Setup_Remove_4](../images/omnipod/RileyLink_Setup_Remove_4.png)

### Add Currently Selected Pod Communication Device (RileyLink)

This process will show how to add a new pod communication device to the Omnipod Driver settings.

1. Under **RileyLink Configuration** press the **Not Set (1)** text to open the **RileyLink Selection** menu.

   > ![RileyLink_Setup_Add_1](../images/omnipod/RileyLink_Setup_Add_1.png)

2. Press the **Scan (2)** button to start scanning for all available Bluetooth devices.

   > ![RileyLink_Setup_Add_2](../images/omnipod/RileyLink_Setup_Add_2.png)

3. Select **your RileyLink (3)** from the list of available devices and you will be returned to the **Omnipod Settings** menu displaying the **MAC Address (4)** of your newly selected device.  Congratulations you have successfully selected your pod communication device.

   > ![RileyLink_Setup_Add_3](../images/omnipod/RileyLink_Setup_Add_3.png) ![RileyLink_Setup_Add_4](../images/omnipod/RileyLink_Setup_Add_4.png)

## Onglet Actions (ACT)

This tab is well documented in the main AAPS documentation but there are a few items on this tab that are specific to how the Omnipod pod differs from tube based pumps, especially after the processes of applying a new pod.

1. Allez dans l'onglet **Actions (ACT)** dans l'interface principale AAPS.
2. Under the **Careportal (1)** section the following 3 fields will have their **age reset** to 0 days and 0 hours **after each pod change**: **Insulin** and **Cannula**. C'est dû à la façon dont la pompe Omnipod est construite et opérationnelle. La **pile de la pompe** et le **réservoir d'insuline** sont intégrés à l'intérieur de chaque pod. Puisque le pod insère la canule directement dans la peau au niveau du site d'application pod, il n'y a pas de tubulure traditionnelle dans les pompes Omnipod. *Par conséquent, après un changement de pod, l'âge de chacune de ces valeurs sera automatiquement réinitialisé à zéro.* **L'âge de la pile pompe** n'est pas indiqué car la durée de vie de la pile du pod sera toujours plus grande que celle du pod (maximum 80 heures).

> ![Actions_Tab](../images/omnipod/Actions_Tab.png)

### Levels

**Niveau d'insuline**

Reporting of the amount of insulin in the Omnipod Eros Pod is not exact.  This is because it is not known exactly how much insulin was put in the pod, only that when the 2 beeps are triggered while filling the pod that over 85 units have been injected. A Pod can hold a maximum of 200 units. Priming can also introduce variance as it is not and exact process.  With both of these factors, the Omnipod driver has been written to give the best approximation of insulin remaining in the reservoir.

> - **Above 50 Units** - Reports a value of 50+U when more than 50 units are currently in the reservoir.
> - **Below 50 Units** - Reports an approximate calculated value of insulin remaining in the reservoir.
> - **SMS** - Renvoie la valeur ou 50+U pour les réponses SMS
> - **Nightscout** - Envoie la valeur 50 vers Nightscout s'il y a plus de 50 unités (version 14,07 et plus).  Les nouvelles versions afficheront la valeur de plus de 50+ si au-dessus de 50 unités.

**Battery Level**

Battery level reporting is a setting that can be enabled to return the current battery level of pod communication devices, such as the OrangeLink, EmaLink or DiaLink.  The RileyLink hardware is not capable of reporting its battery level.  The battery level is reported after each communication with the pod, so when charging a linear increase may not be observed.  A manual refresh will update the current battery level.  When a supported Pod communication device is disconnected a value of 0% will be reported.

> - **RileyLink hardware is NOT capable of reporting battery level**
> - **"Show battery level reported by OrangeLink/EmaLink/DiaLink" Setting MUST be enabled in the Omnipod settings to report battery level values**
> - **Battery level reporting ONLY works for OrangeLink, EmaLink and DiaLink Devices**
> - **Battery Level reporting MAY work for other devices (excluding RileyLink)**
> - **SMS** - Returns current battery level as a response when an actual level exists, a value of n/a will not be returned
> - **Nightscout** - Battery level is reported when an actual level exists, a value of n/a will not be reported

(OmnipodEros-troubleshooting)=

## Résolution de problèmes

### Erreurs Pod

Les pods échouent occasionnellement en raison de problèmes variés, y compris des problèmes matériels avec le Pod lui-même. Il est préférable de ne pas les appeler dans Insulet, car l'utilisation d'AAPS n'est pas approuvée. A list of fault codes can be found [here](https://github.com/openaps/openomni/wiki/Fault-event-codes) to help determine the cause.

### Empêcher l'erreur 49 échecs du pod

Cet échec est lié à un état du pod incorrect pour une commande ou à une erreur lors d'une commande d'injection d'insuline. We recommend users to switch to the Nightscout client to *upload only (Disable sync)* under the **Config Builder**➜**General**➜**NSClient**➜**cog wheel**➜**Advanced Settings** to prevent possible failures.

### Alertes Pompe hors de portée

It is recommended that pump unreachable alerts be configured to **120 minutes** by going to the top right-hand side three-dot menu, selecting **Preferences**➜**Local Alerts**➜**Pump unreachable threshold \[min\]** and setting this to **120**.

(OmnipodEros-import-settings-from-previous-aaps)=
### Import Settings from previous AAPS

Please note that importing settings has the possibility to import an outdated Pod status. As a result, you may lose an active Pod. It is therefore strongly recommended that you **do not import settings while on an active Pod session**.

1. Deactivate your pod session. Verify that you do not have an active pod session.
2. Exportez vos paramètres et stockez en une copie dans un endroit sûr.
3. Uninstall the previous version of AAPS and restart your phone.
4. Install the new version of AAPS and verify that you do not have an active pod session.
5. Import your settings and activate your new pod.

### Alertes Pilote Omnipod

please note that the Omnipod driver presents a variety of unique alerts on the **Overview tab**, most of them are informational and can be dismissed while some provide the user with an action to take to resolve the cause of the triggered alert. Un résumé des principales alertes que vous pouvez rencontrer est listé ci-dessous:

#### No active Pod

No active Pod session detected. Cette alerte peut être temporairement rejetée en appuyant sur **REPORT ALARME** mais elle se déclenchera tant qu'un nouveau pod n'a pas été activé. Once activated this alert is automatically silenced.

#### Pod suspended

Informational alert that Pod has been suspended.

#### Setting basal profile failed. Delivery might be suspended! Veuillez actualiser manuellement l'état du Pod à partir de l'onglet Omnipod et reprendre l'injection si nécessaire..

Alerte informative que le réglage du profil basal du Pod a échoué, et vous devrez appuyer sur *Actualiser* dans l'onglet Omnipod.

#### Impossible de vérifier si le bolus SMB a réussi. Si vous êtes sûr que le Bolus n'a pas réussi, vous devez supprimer manuellement l'entrée SMB dans les traitements.

Alert that the SMB bolus success could not be verified, you will need to verify the *Last bolus* field on the Omnipod tab to see if SMB bolus succeeded and if not remove the entry from the Treatments tab.

#### Incertain si l'action "Bolus/DBT/SMB" est terminée, merci de vérifier manuellement s'il a réussi.

Due to the way that the RileyLink and Omnipod communicate, situations can occur where it is *uncertain* if a command was successfully processed. The need to inform the user of this uncertainty was necessary.

Below are a few examples of when an uncertain notification can occur.

- **Boluses** - Uncertain boluses cannot be automatically verified. The notification will remain until the next bolus but a manual pod refresh will clear the message. *By default alerts beeps are enabled for this notification type as the user will manually need to verify them.*
- **TBRs, Pod Statuses, Profile Switches, Time Changes** - a manual pod refresh will clear the message. By default alert beeps are disabled for this notification type.
- **Pod Time Deviation -** When the time on the pod and the time your phone deviates too much then it is difficult for AAPS loop to function and make accurate predictions and dosage recommendations. If the time deviation between the pod and the phone is more than 5 minutes then AAPS will report the pod is in a Suspended state under Pod status with a HANDLE TIME CHANGE message. An additional **Set Time** icon will appear at the bottom of the Omnipod (POD) tab. Clicking Set Time will synchronize the time on the pod with the time on the phone and then you can click the RESUME DELIVERY button to continue normal pod operations.

## Best Practices

(OmnipodEros-optimal-omnipod-and-rileylink-positioning)=

### Optimal Omnipod and RileyLink Positioning

The antenna used on the RileyLink to communicate with an Omnipod pod is a 433 MHz helical spiral antenna. Due to its construction properties it radiates an omni directional signal like a three dimensional doughnut with the z-axis representing the vertical standing antenna. This means that there are optimal positions for the RileyLink to be placed, especially during pod activation and deactivation routines.

![Toroid_w_CS](../images/omnipod/Toroid_w_CS.png)

> *(Fig 1. Graphical plot of helical spiral antenna in an omnidirectional pattern*)

Because of both safety and security concerns, pod *activation* has to be done at a range *closer (~30 cm away or less)* than other operations such as giving a bolus, setting a TBR or simply refreshing the pod status. Due to the nature of the signal transmission from the RileyLink antenna it is NOT recommended to place the pod directly on top of or right next to the RileyLink.

The image below shows the optimal way to position the RileyLink during pod activation and deactivation procedures. The pod may activate in other positions but you will have the most success using the position in the image below.

*Note: If after optimally positioning the pod and RileyLink communication fails, this may be due to a low battery which decreases the transmission range of the RileyLink antenna. To avoid this issue make sure the RileyLink is properly charged or connected directly to a charging cable during this process.*

![Omnipod_pod_and_RileyLink_Position](../images/omnipod/Omnipod_pod_and_RileyLink_Position.png)

## Where to get help for Omnipod driver

All of the development work for the Omnipod driver is done by the community on a volunteer basis; we ask that you please be considerate and use the following guidelines when requesting assistance:

- **Niveau 0 :** Lisez la section correspondante de cette documentation pour vous assurer que vous comprenez comment la fonctionnalité avec laquelle vous avez des difficultés est censée fonctionner.
- **Niveau 1 :** Si vous rencontrez toujours des problèmes que vous n'arrivez pas à résoudre en utilisant ce document, alors veuillez aller sur la chaine *#androidaps* sur **Discord** avec [ce lien d'invitation](https://discord.gg/4fQUWHZ4Mw).
- **Level 2:** Search existing issues to see if your issue has already been reported; if not, please create a new [issue](https://github.com/nightscout/AndroidAPS/issues) and attach your [log files](../Usage/Accessing-logfiles.md).
- **Soyez patient - la plupart des membres de notre communauté sont des bénévoles de bonne nature, et résoudre les problèmes nécessite souvent du temps et de la patience de la part des utilisateurs et des développeurs.**
