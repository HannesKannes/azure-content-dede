<properties 
	pageTitle="Konfigurieren von Software-RAID auf einem virtuellen Linux-Computer | Microsoft Azure" 
	description="Erfahren Sie, wie Sie mdadm zum Konfigurieren von RAID unter Linux in Azure verwenden." 
	services="virtual-machines-linux" 
	documentationCenter="na" 
	authors="rickstercdn"  
	manager="timlt" 
	editor="tysonn"
	tag="azure-service-management,azure-resource-manager" />

<tags 
	ms.service="virtual-machines-linux" 
	ms.workload="infrastructure-services" 
	ms.tgt_pltfrm="vm-linux" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/06/2016" 
	ms.author="rclaus"/>



# Konfigurieren von Software-RAID unter Linux
Ein häufiges Szenario ist die Verwendung von Software-RAID auf virtuellen Linux-Computern in Azure, um mehrere angefügte Datenträger als einzelnes RAID-Gerät darzustellen. Dies kann normalerweise angewendet werden, um die Leistung zu verbessern und optimierten Durchsatz im Vergleich zur Verwendung eines einzelnen Datenträgers zu ermöglichen.


## Anfügen von Datenträgern
Es sind zwei oder mehr leere Datenträger erforderlich, um ein RAID-Gerät zu konfigurieren. Der Hauptgrund für die Erstellung eines RAID-Geräts ist die Leistungsverbesserung Ihrer Datenträger-E/A. Basierend auf Ihren E/A-Anforderungen können Sie wählen, in unserem Standardspeicher gespeicherte Datenträger mit bis zu 500 IOPS und Datenträger anzufügen oder in unserem Premiumspeicher gespeicherte Datenträger mit bis zu 5000 IOPS und Datenträger anzufügen. In diesem Artikel wird nicht erläutert, wie Sie einem virtuellen Linux-Computer Datenträger bereitstellen und an diesen anfügen. Eine ausführliche Anleitung, wie Sie einen leeren Datenträger an einen virtuellen Linux-Computer in Azure anfügen, finden Sie im Microsoft Azure-Artikel [Anfügen eines Datenträgers](virtual-machines-linux-add-disk.md).


## Installieren des mdadm-Dienstprogramms

- **Ubuntu**

		# sudo apt-get update
		# sudo apt-get install mdadm

- **CentOS und Oracle Linux**

		# sudo yum install mdadm

- **SLES und openSUSE**

		# zypper install mdadm


## Erstellen der Datenträgerpartitionen
In diesem Beispiel erstellen wir eine einzelne Datenträgerpartition unter „/dev/sdc“. Die neue Datenträgerpartition wird „/dev/sdc1“ genannt.

1. Starten Sie "fdisk", um mit dem Erstellen der Partitionen zu beginnen.

		# sudo fdisk /dev/sdc
		Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
		Building a new DOS disklabel with disk identifier 0xa34cb70c.
		Changes will remain in memory only, until you decide to write them.
		After that, of course, the previous content won't be recoverable.

		WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
				 switch off the mode (command 'c') and change display units to
				 sectors (command 'u').

2. Drücken Sie an der Eingabeaufforderung 'n', um eine **n**eue Partition zu erstellen:

		Command (m for help): n

3. Drücken Sie anschließend 'p', um ein **p**rimäre Partition zu erstellen:

		Command action
			e   extended
			p   primary partition (1-4)

4. Drücken Sie '1', um Partitionsnummer 1 auszuwählen:

		Partition number (1-4): 1

5. Wählen Sie den Ausgangspunkt der neuen Partition, oder drücken Sie `<enter>`, um die Standardeinstellung zu akzeptieren und die Partition am Anfang des freien Speicherplatzes auf dem Laufwerk zu platzieren:

		First cylinder (1-1305, default 1):
		Using default value 1

6. Legen Sie die Größe der Partition fest, z. B. '+10G', um eine 10-Gigabyte-Partition zu erstellen. Oder drücken Sie einfach `<enter>`, um eine einzelne Partition zu erstellen, die das gesamte Laufwerk umfasst:

		Last cylinder, +cylinders or +size{K,M,G} (1-1305, default 1305): 
		Using default value 1305

7. Ändern Sie anschließend die ID und den Partitions**t**yp von der Standard-ID '83' (Linux) in ID 'fd' (Linux-RAID automatisch):

		Command (m for help): t
		Selected partition 1
		Hex code (type L to list codes): fd

8. Zum Abschluss schreiben Sie die Partitionstabelle in das Laufwerk und beenden "fdisk":

		Command (m for help): w
		The partition table has been altered!


## Erstellen des RAID-Arrays

1. Im folgenden Beispiel werden drei Partitionen auf drei separaten Datenträgern (sdc1, sdd1, sde1) zu einem RAID (Level 0) verbunden. Nach Ausführung dieses Befehls wird ein neues RAID namens **/dev/md127** erstellt. Beachten Sie außerdem Folgendes: Falls diese Datenträger zuvor Teil eines anderen, nicht mehr bestehenden RAID-Arrays waren, muss der Befehl `mdadm` unter Umständen mit dem Parameter `--force` versehen werden:

		# sudo mdadm --create /dev/md127 --level 0 --raid-devices 3 \
		  /dev/sdc1 /dev/sdd1 /dev/sde1

2. Erstellen des Dateisystems auf dem neuen RAID-Gerät

	**CentOS, Oracle Linux, SLES 12, openSUSE und Ubuntu**

		# sudo mkfs -t ext4 /dev/md127

	**SLES 11**

		# sudo mkfs -t ext3 /dev/md127

	**SLES 11 und openSUSE** – "boot.md" aktivieren und "mdadm.conf" erstellen

		# sudo -i chkconfig --add boot.md
		# sudo echo 'DEVICE /dev/sd*[0-9]' >> /etc/mdadm.conf

	>[AZURE.NOTE] Nach den Änderungen an den SUSE-Systemen kann ein Neustart erforderlich sein. Dieser Schritt ist für SLES 12 *nicht* erforderlich.


## Hinzufügen des neuen Laufwerks zu "/etc/fstab"

**Achtung:** Eine fehlerhafte Bearbeitung der Datei /etc/fstab kann dazu führen, dass das System sich nicht mehr starten lässt. Wenn Sie sich nicht sicher sind, helfen Ihnen die Informationen zur richtigen Bearbeitung dieser Datei in der Dokumentation weiter. Außerdem wird empfohlen, ein Backup der Datei /etc/fstab zu erstellen, bevor Sie sie bearbeiten.

1. Erstellen Sie den gewünschten Bereitstellungspunkt für das neue Dateisystem, zum Beispiel:

		# sudo mkdir /data

2. Beim Bearbeiten von /etc/fstab sollte die **UUID** verwendet werden, um auf das Dateisystem zu verweisen statt auf den Gerätenamen. Verwenden Sie das Hilfsprogramm `blkid`, um die UUID für das neue Dateisystem zu bestimmen:

		# sudo /sbin/blkid
		...........
		/dev/md127: UUID="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" TYPE="ext4"

3. Öffnen Sie "/etc/fstab" in einem Texteditor, und fügen Sie einen Eintrag für das neue Dateisystem hinzu, z. B.:

		UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults  0  2

	Oder für **SLES 11 und openSUSE**:

		/dev/disk/by-uuid/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext3  defaults  0  2

	Speichern und schließen Sie anschließend /etc/fstab.

4. Testen Sie, ob der Eintrag für /etc/fstab korrekt ist:

		# sudo mount -a

	Wenn dieser Befehl zu einer Fehlermeldung führt, überprüfen Sie die Syntax in der Datei „/etc/fstab“.

	Führen Sie nun den Befehl `mount` aus, um sicherzustellen, dass das Dateisystem eingebunden wurde:

		# mount
		.................
		/dev/md127 on /data type ext4 (rw)

5. (Optional) Failsafe-Startparameter

	**fstab-Konfiguration**

	Viele Distributionen beinhalten den Bereitstellungsparameter `nobootwait` oder `nofail`, der der Datei „/etc/fstab“ hinzugefügt werden kann. Diese Parameter lassen Fehler beim Bereitstellen eines bestimmten Dateisystems zu und ermöglichen dem Linux-System das Fortsetzen des Startvorgangs, selbst wenn das RAID-Dateisystem nicht ordnungsgemäß geladen werden kann. Weitere Informationen zu diesen Parametern erhalten Sie in der Dokumentation der Distribution.

	Beispiel (Ubuntu):

		UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,nobootwait  0  2

	**Linux-Startparameter**

	Zusätzlich zu den oben angegebenen Parametern kann der Kernelparameter „`bootdegraded=true`“ das Starten des Systems ermöglichen, selbst wenn das RAID-System beschädigt oder beeinträchtigt ist, weil beispielsweise unabsichtlich ein Datenträger aus dem virtuellen Computer entfernt wurde. Normalerweise kann dies auch den Start des Systems verhindern.

	Informationen zum Bearbeiten der Kernel-Parameter erhalten Sie in der Dokumentation der Distribution. Beispielsweise können diese Parameter der Datei „`/boot/grub/menu.lst`“ in vielen Distributionen (CentOS, Oracle Linux, SLES 11) manuell hinzugefügt werden. In Ubuntu kann dieser Parameter der Variablen `GRUB_CMDLINE_LINUX_DEFAULT` unter „/etc/default/grub“ hinzugefügt werden.

<!---HONumber=AcomDC_0914_2016-->