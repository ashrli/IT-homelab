# 00 – Lab Grundaufbau

## Ziel
Simulation einer virtuellen Arbeitsumgebung basierend 
auf Windows Server und Active Directory.

## Aufbau
- Oracle VirtualBox als Virtualisierungsprogramm
- Windows Server 2025 ISO (Domänencontroller)
- Windows 11 Enterprise LTSC ISO (Client)
- Server-VM: 8 GB RAM, 40 GB Speicher (dynamisch)
- Client-VM: 4 GB RAM, 64 GB Speicher (dynamisch)

## Schritte
1. Oracle VirtualBox als Basisprogramm installiert
2. Windows Server 2025 ISO und Windows 11 Enterprise 
   LTSC ISO als Installationsmedien eingebunden
3. Server-VM angelegt und konfiguriert 
   (8 GB RAM, 40 GB, 4 CPU-Kerne)
4. Client-VM angelegt und konfiguriert 
   (4 GB RAM, 64 GB, 2 CPU-Kerne)

## Entscheidungen
**Ursprünglich:** Proxmox als Virtualisierungslösung, 
da empfohlen für professionelle Umgebungen.

**Neue Entscheidung:** Proxmox wird vorerst 
übersprungen. Proxmox wurde zunächst innerhalb von 
VirtualBox installiert (verschachtelte 
Virtualisierung / Nested Virtualization). Dabei 
stellte sich heraus, dass diese Architektur 
ressourcentechnisch deutlich mehr verbraucht als 
nötig, schwerer zu debuggen ist und vermeidbare 
Fehler erzeugt. Oracle VirtualBox übernimmt 
vorerst die Rolle des Hypervisors.

**Ausblick:** Proxmox wird später auf einem 
separaten, dedizierten Gerät direkt auf der 
Hardware installiert – so wie es in echten 
Umgebungen gemacht wird.

## Probleme & Lösung
**Problem 1 – Nested Virtualization:**
Proxmox lief innerhalb von VirtualBox, was zu 
zwei Virtualisierungsebenen übereinander führte. 
Fehler häuften sich und die Performance war 
inakzeptabel.
Lösung: Proxmox-Ebene entfernt, VMs laufen 
direkt in VirtualBox.

**Problem 2 – AMD-V deaktiviert:**
Fehlermeldung beim VM-Start: 
„Not in a hypervisor partition (HVP=0), 
AMD-V disabled."
Ursache: Hardware-Virtualisierung war im BIOS 
deaktiviert.
Lösung: BIOS geöffnet (Entf-Taste beim Start) → 
Tweaker → Advanced Frequency Settings → 
Advanced CPU Core Settings → 
SVM Mode auf „Enabled" gesetzt → 
gespeichert (F10) → neu gestartet.

## Gelernt
- Was verschachtelte Virtualisierung ist und 
  warum sie Overhead erzeugt
- Dass die einfachste Architektur, die den Zweck 
  erfüllt, meist die beste ist
- Was AMD-V / SVM Mode ist und warum 
  Hardware-Virtualisierung im BIOS aktiviert 
  sein muss
- Wie man im Gigabyte Aorus BIOS navigiert