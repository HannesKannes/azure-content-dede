<properties 
    pageTitle="Tutorial: Azure Active Directory-Integration mit ShiftPlanning | Microsoft Azure" 
    description="Erfahren Sie, wie Sie ShiftPlanning mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen." 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="07/07/2016" 
    ms.author="jeedes" />

#Tutorial: Azure Active Directory-Integration mit ShiftPlanning
  
In diesem Tutorial wird die Integration von Azure und ShiftPlanning erläutert. Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

-   Ein gültiges Azure-Abonnement
-   Ein ShiftPlanning-Abonnement, für das einmaliges Anmelden aktiviert ist
  
Nach Abschluss dieses Tutorials können sich die ShiftPlanning zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf Ihrer ShiftPlanning-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).
  
Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1.  Aktivieren der Anwendungsintegration für ShiftPlanning
2.  Konfigurieren der einmaligen Anmeldung
3.  Konfigurieren der Benutzerbereitstellung
4.  Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-shiftplanning-tutorial/IC786612.png "Szenario")
##Aktivieren der Anwendungsintegration für ShiftPlanning
  
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für ShiftPlanning aktivieren.

###So aktivieren Sie die Anwendungsintegration für ShiftPlanning:

1.  Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory](./media/active-directory-saas-shiftplanning-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

    ![Anwendungen](./media/active-directory-saas-shiftplanning-tutorial/IC700994.png "Anwendungen")

4.  Klicken Sie unten auf der Seite auf **Hinzufügen**.

    ![Anwendung hinzufügen](./media/active-directory-saas-shiftplanning-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-shiftplanning-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")

6.  Geben Sie in das **Suchfeld** **ShiftPlanning** ein.

    ![Anwendungskatalog](./media/active-directory-saas-shiftplanning-tutorial/IC786613.png "Anwendungskatalog")

7.  Wählen Sie im Ergebnisbereich **ShiftPlanning** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

    ![ShiftPlanning](./media/active-directory-saas-shiftplanning-tutorial/IC786614.png "ShiftPlanning")
##Konfigurieren der einmaligen Anmeldung
  
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei ShiftPlanning zu authentifizieren. Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei erstellen. Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren eines binären Zertifikats in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o) weitere Informationen.

###So konfigurieren Sie einmaliges Anmelden

1.  Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **ShiftPlanning** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-shiftplanning-tutorial/IC786615.png "Einmaliges Anmelden konfigurieren")

2.  Wählen Sie auf der Seite **Wie sollen sich Benutzer bei ShiftPlanning anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-shiftplanning-tutorial/IC786616.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **ShiftPlanning-Anmelde-URL** Ihre URL im Format „*https://company.shiftplanning.com/includes/saml/*" ein, und klicken Sie dann auf **Weiter**.

    ![App-URL konfigurieren](./media/active-directory-saas-shiftplanning-tutorial/IC786617.png "App-URL konfigurieren")

4.  Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für ShiftPlanning** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-shiftplanning-tutorial/IC786618.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie sich in einem anderen Webbrowserfenster bei der **ShiftPlanning**-Unternehmenswebsite als Administrator an.

6.  Klicken Sie oben im Menü auf **Administrator**.

    ![Admin](./media/active-directory-saas-shiftplanning-tutorial/IC786619.png "Admin")

7.  Klicken Sie unter **Integration**, klicken Sie auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden](./media/active-directory-saas-shiftplanning-tutorial/IC786620.png "Einmaliges Anmelden")

8.  Führen Sie im Abschnitt **Einmaliges Anmelden** die folgenden Schritte aus:

    ![Einmaliges Anmelden](./media/active-directory-saas-shiftplanning-tutorial/IC786905.png "Einmaliges Anmelden")

    1.  Wählen Sie **SAML aktiviert** aus.
    2.  Wählen Sie **Anmeldung mit Kennwort zulassen** aus.
    3.  Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für ShiftPlanning** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **SAML Issuer URL** ein.
    4.  Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für ShiftPlanning** den Wert der **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Remote Logout URL** ein.
    5.  Erstellen Sie eine **Base-64-codierte** Datei aus dem heruntergeladenen Zertifikat.

        >[AZURE.TIP]Weitere Informationen finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o) (in englischer Sprache).

    6.  Öffnen Sie das Base-64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **X.509-Zertifikat** ein.
    7.  Klicken Sie auf **Einstellungen speichern**.

9.  Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-shiftplanning-tutorial/IC786621.png "Einmaliges Anmelden konfigurieren")
##Konfigurieren der Benutzerbereitstellung
  
Damit sich Azure AD-Benutzer bei ShiftPlanning anmelden können, müssen sie in ShiftPlanning bereitgestellt werden. Im Fall von ShiftPlanning ist die Bereitstellung eine manuelle Aufgabe.

###So stellen Sie Benutzerkonten bereit:

1.  Melden Sie sich bei der **ShiftPlanning**-Unternehmenswebsite als Administrator an.

2.  Klicken Sie auf **Administrator**.

    ![Admin](./media/active-directory-saas-shiftplanning-tutorial/IC786619.png "Admin")

3.  Klicken Sie auf **Personal**.

    ![Mitarbeiter](./media/active-directory-saas-shiftplanning-tutorial/IC786623.png "Mitarbeiter")

4.  Klicken Sie unter **Aktionen** auf **Mitarbeiter hinzufügen**.

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-shiftplanning-tutorial/IC786624.png "Mitarbeiter hinzufügen")

5.  Führen Sie im Abschnitt **Mitarbeiter hinzufügen** die folgenden Schritte aus:

    ![Mitarbeiter speichern](./media/active-directory-saas-shiftplanning-tutorial/IC786625.png "Mitarbeiter speichern")

    1.  Geben Sie den **Vornamen**, den **Nachnamen** und die **Email-Adresse** eines gültigen AAD-Benutzerkontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein.
    2.  Klicken Sie auf **Mitarbeiter speichern**.

>[AZURE.NOTE]Sie können AAD-Benutzerkonten auch mithilfe von anderen Tools zum Erstellen von ShiftPlanning-Benutzerkonten oder mithilfe der von ShiftPlanning bereitgestellten APIs erstellen.

##Zuweisen von Benutzern
  
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

###So weisen Sie ShiftPlanning Benutzer zu:

1.  Erstellen Sie im klassischen Azure-Portal ein Testkonto.

2.  Klicken Sie auf der Anwendungsintegrationsseite für **ShiftPlanning** auf **Benutzer zuweisen**.

    ![Benutzer zuweisen](./media/active-directory-saas-shiftplanning-tutorial/IC786626.png "Benutzer zuweisen")

3.  Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.

    ![Ja](./media/active-directory-saas-shiftplanning-tutorial/IC767830.png "Ja")
  
Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

<!---HONumber=AcomDC_0713_2016-->