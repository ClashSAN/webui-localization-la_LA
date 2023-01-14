Dies ist eine Feature-Showcase-Seite für die Web-Benutzeroberfläche von Stable Diffusion .

Alle Beispiele sind nicht-herausgepickt, sofern nicht anders angegeben.

Alt-Diffusion
Das Modell ist darauf trainiert, Eingaben in verschiedenen Sprachen zu akzeptieren. Weitere Informationen: https://arxiv.org/abs/2211.06679

Laden Sie den Checkpoint von drive.filen.io herunter
lege es ins models/Stable-DiffusionVerzeichnis
holen Sie sich die Konfiguration von configs/alt-diffusion-inference.yamlund legen Sie sie an der gleichen Stelle wie der Checkpoint ab, und benennen Sie sie so um, dass sie denselben Dateinamen hat (dh wenn Ihr Checkpoint heißt ad.ckpt, sollte die Konfiguration benannt werden ad.yaml)
Wählen Sie den neuen Kontrollpunkt in der Benutzeroberfläche aus
Mechanisch wird der Aufmerksamkeits-/Hervorhebungsmechanismus (siehe unten in den Funktionen) unterstützt, scheint aber viel weniger Wirkung zu haben, wahrscheinlich aufgrund der Art und Weise, wie Alt-Diffusion implementiert ist. Das Überspringen von Clips wird nicht unterstützt, die Einstellung wird ignoriert.

Weitere Informationen finden Sie in der PR: https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/5238

Stalldiffusion 2.0
Basismodelle
Modelle werden unterstützt: 768-v-ema.ckpt ( model , config ) und 512-base-ema.ckpt ( model , config ). 2.1 Checkpoints sollten auch funktionieren.

Laden Sie den Checkpoint herunter (von hier: https://huggingface.co/stabilityai/stable-diffusion-2 )
lege es ins models/Stable-DiffusionVerzeichnis
Holen Sie sich die Konfiguration aus dem SD2.0-Repository und legen Sie sie an derselben Stelle wie den Checkpoint ab, und benennen Sie sie so um, dass sie denselben Dateinamen hat (dh wenn Ihr Checkpoint heißt 768-v-ema.ckpt, sollte die Konfiguration benannt werden 768-v-ema.yaml)
Wählen Sie den neuen Kontrollpunkt in der Benutzeroberfläche aus
Die Zuglasche wird höchstwahrscheinlich für die 2.0-Modelle kaputt sein.

Wenn 2.0 oder 2.1 schwarze Bilder erzeugt, aktivieren Sie die volle Präzision mit --no-halfoder versuchen Sie es mit der --xformersOptimierung.

Hinweis: SD 2.0 und 2.1 reagieren aufgrund ihres neuen Cross-Attention-Modulsempfindlicher auf die numerische Instabilität von FP16 (wie von ihnen selbst hier angemerkt).

Auf fp16: Kommentar zum Aktivieren in webui-user.bat:

@echo off

set PYTHON=
set GIT=
set VENV_DIR=
set COMMANDLINE_ARGS=your command line options
set STABLE_DIFFUSION_COMMIT_HASH="c12d960d1ee4f9134c2516862ef991ec52d3f59e"
set ATTN_PRECISION=fp16

call webui.bat
Tiefengeführtes Modell
Weitere Informationen . PR . Anweisungen:

Laden Sie den Checkpoint 512-deep-ema.ckpt herunter
platzieren Sie es in Modellen/Stable-Diffusion
Nehmen Sie die Konfiguration und legen Sie sie im selben Ordner wie den Checkpoint ab
Benennen Sie die Konfiguration um in512-depth-ema.yaml
Wählen Sie den neuen Kontrollpunkt in der Benutzeroberfläche aus
Das tiefengeführte Modell funktioniert nur auf der Registerkarte img2img.

Übermalen
Outpainting erweitert das Originalbild und übermalt den entstandenen leeren Raum.

Beispiel:

Original	Übermalen	Mal wieder übermalen
		
Originalbild von anonymem Benutzer von 4chan. Danke, anonymer Benutzer.

Sie finden die Funktion im img2img-Tab ganz unten unter Script -> Poor man's outpainting.

Outpainting scheint im Gegensatz zur normalen Bilderzeugung sehr von einer großen Schrittzahl zu profitieren. Ein Rezept für ein gutes Outpainting ist eine gute Eingabeaufforderung, die zum Bild passt, Schieberegler für Entrauschen und CFG-Skala auf Maximum eingestellt und eine Schrittzahl von 50 bis 100 mit Euler-Ahnen- oder DPM2-Ahnen-Samplern.

81 Schritte, Euler A	30 Schritte, Euler A	10 Schritte, Euler A	80 Stufen, Euler A
			
Malen
Zeichnen Sie auf der Registerkarte img2img eine Maske über einen Teil des Bildes, und dieser Teil wird eingemalt.



Optionen zum Einfärben:

im Webeditor selbst eine Maske zeichnen
Löschen Sie einen Teil des Bildes in einem externen Editor und laden Sie ein transparentes Bild hoch. Auch leicht transparente Bereiche werden Teil der Maske. Beachten Sie, dass einige Editoren vollständig transparente Bereiche standardmäßig als schwarz speichern.
ändere den Modus (unten rechts im Bild) auf "Maske hochladen" und wähle ein separates Schwarz-Weiß-Bild für die Maske (weiß = inpaint).
Modell bemalen
RunwayML hat ein zusätzliches Modell trainiert, das speziell für das Inpainting entwickelt wurde. Dieses Modell akzeptiert zusätzliche Eingaben - das anfängliche Bild ohne Rauschen plus die Maske - und scheint bei der Arbeit viel besser zu sein.

Download und zusätzliche Informationen für das Modell finden Sie hier: https://github.com/runwayml/stable-diffusion#inpainting-with-stable-diffusion

Um das Modell zu verwenden, müssen Sie den Prüfpunkt so umbenennen, dass sein Dateiname auf endet inpainting.ckpt, z. B. 1.5-inpainting.ckpt.

Wählen Sie danach einfach den Kontrollpunkt aus, wie Sie normalerweise jeden Kontrollpunkt auswählen würden, und Sie können loslegen.

Maskierter Inhalt
Das maskierte Inhaltsfeld bestimmt, dass Inhalt platziert wird, um ihn in die maskierten Bereiche zu platzieren, bevor sie eingefärbt werden.

Maske	füllen	Original	latentes Rauschen	latent nichts
				
Inpaint-Bereich
Normalerweise ändert das Inpainting das Bild auf die Zielauflösung, die in der Benutzeroberfläche angegeben ist. Bei Inpaint area: Only masked aktivierter Option wird nur die Größe des maskierten Bereichs geändert und nach der Verarbeitung wieder in das Originalbild eingefügt. Dadurch können Sie mit großen Bildern arbeiten und das eingemalte Objekt mit einer viel höheren Auflösung rendern.

Eingang	Inpaint-Bereich: Ganzes Bild	Inpaint-Bereich: Nur maskiert
		
Maskierungsmodus
Es gibt zwei Optionen für den maskierten Modus:

Maskiert einfärben - Der Bereich unter der Maske wird eingefärbt
Inpaint nicht maskiert - unter der Maske bleibt unverändert, alles andere ist inpaint
Alpha-Maske
Eingang	Ausgabe
	
Prompt-Matrix
Trennen Sie mehrere Eingabeaufforderungen mithilfe des |Zeichens, und das System erstellt ein Bild für jede Kombination davon. Wenn Sie beispielsweise die a busy city street in a modern city|illustration|cinematic lightingEingabeaufforderung verwenden, sind vier Kombinationen möglich (der erste Teil der Eingabeaufforderung wird immer beibehalten):

a busy city street in a modern city
a busy city street in a modern city, illustration
a busy city street in a modern city, cinematic lighting
a busy city street in a modern city, illustration, cinematic lighting
Es werden vier Bilder in dieser Reihenfolge erstellt, alle mit demselben Startwert und jeweils mit einer entsprechenden Eingabeaufforderung: 

Ein weiteres Beispiel, diesmal mit 5 Prompts und 16 Variationen: 

Sie finden die Funktion ganz unten unter Skript -> Eingabeaufforderungsmatrix.

Farbskizze
Grundlegendes Malwerkzeug für img2img. Um diese Funktion in img2img zu verwenden, aktivieren Sie sie mit --gradio-img2img-tool color-sketchin commandline args. Um diese Funktion im Inpainting-Modus zu verwenden, aktivieren Sie mit --gradio-inpaint-tool color-sketch. Chromium-basierte Browser unterstützen ein Dropper-Tool. (siehe Bild)

Tropfer

Stabile Diffusion der gehobenen Klasse
Hochskalieren Sie das Bild mit RealESRGAN/ESRGAN und gehen Sie dann die Kacheln des Ergebnisses durch und verbessern Sie sie mit img2img. Es hat auch eine Option, mit der Sie den Upscaling-Teil selbst in einem externen Programm durchführen und einfach mit img2img die Kacheln durchgehen können.

Ursprüngliche Idee von: https://github.com/jquesnelle/txt2imghd . Dies ist eine unabhängige Implementierung.

Um diese Funktion zu verwenden, wählen Sie SD upscale from the scripts dropdown selection(img2img tab).

chrome_dl8hcMPYcx

Das Eingabebild wird auf das Doppelte der ursprünglichen Breite und Höhe hochskaliert, und die Schieberegler für Breite und Höhe der Benutzeroberfläche geben die Größe der einzelnen Kacheln an. Aufgrund der Überlappung kann die Größe der Kachel sehr wichtig sein: Ein 512 x 512-Bild benötigt neun 512 x 512-Kacheln (wegen der Überlappung), aber nur vier 640 x 640-Kacheln.

Empfohlene Parameter für die Hochskalierung:

Stichprobenverfahren: Euler a
Denoising-Stärke: 0,2, kann auf 0,4 steigen, wenn Sie abenteuerlustig sind
Original	EchtESRGAN	Topas Gigapixel	SD-gehoben
			
			
			
Aufmerksamkeit/Betonung
Die Verwendung ()in der Eingabeaufforderung erhöht die Aufmerksamkeit des Modells für eingeschlossene Wörter und []verringert sie. Sie können mehrere Modifikatoren kombinieren:



Spickzettel:

a (word)- Aufmerksamkeit um wordden Faktor 1,1 steigern
a ((word))- Aufmerksamkeit um wordden Faktor 1,21 erhöhen (= 1,1 * 1,1)
a [word]- Aufmerksamkeit um wordden Faktor 1,1 verringern
a (word:1.5)- Aufmerksamkeit um wordden Faktor 1,5 steigern
a (word:0.25)- Aufmerksamkeit um wordden Faktor 4 verringern (= 1 / 0,25)
a \(word\)- Verwenden Sie wörtliche ()Zeichen in der Eingabeaufforderung
Mit ()kann ein Gewicht wie folgt angegeben werden: (text:1.4). Wenn das Gewicht nicht angegeben ist, wird es mit 1,1 angenommen. Die Gewichtsangabe funktioniert nur mit ()nicht mit [].

Wenn Sie eines der wörtlichen Zeichen in der Eingabeaufforderung verwenden möchten, verwenden Sie ()[]den umgekehrten Schrägstrich, um sie zu maskieren: anime_\(character\).

Am 29.09.2022 wurde eine neue Implementierung hinzugefügt, die Escape-Zeichen und numerische Gewichtungen unterstützt. Ein Nachteil der neuen Implementierung ist, dass die alte nicht perfekt war und manchmal Zeichen fraß: "a (((farm))), daytime" würde beispielsweise ohne das Komma zu "a farm daytime" werden. Dieses Verhalten wird von der neuen Implementierung nicht geteilt, die den gesamten Text korrekt beibehält, und dies bedeutet, dass Ihre gespeicherten Seeds möglicherweise unterschiedliche Bilder erzeugen. Momentan gibt es in den Einstellungen eine Option, um die alte Implementierung zu verwenden.

NAI verwendet meine Implementierung von vor dem 29.09.2022, außer dass sie 1,05 als Multiplikator haben und {}anstelle von verwenden (). Es gilt also die Umrechnung:

ihr {word}= unser(word:1.05)
ihr {{word}}= unser(word:1.1025)
ihr [word]= unser (word:0.952)(0,952 = 1/1,05)
ihr [[word]]= unser (word:0.907)(0,907 = 1/1,05/1,05)
Schleife
Wenn Sie das Loopback-Skript in img2img auswählen, können Sie das Ausgabebild automatisch als Eingabe für den nächsten Stapel einspeisen. Entspricht dem Speichern des Ausgabebildes und dem Ersetzen des Eingabebildes durch dieses. Die Einstellung für die Stapelanzahl steuert, wie viele Iterationen Sie davon erhalten.

Normalerweise würden Sie dabei eines von vielen Bildern für die nächste Iteration selbst auswählen, daher ist die Nützlichkeit dieser Funktion möglicherweise fraglich, aber ich habe damit einige sehr schöne Ergebnisse erzielt, die ich nicht erhalten konnte Andernfalls.

Beispiel: (herausgepicktes Ergebnis)



Originalbild von anonymem Benutzer von 4chan. Danke, anonymer Benutzer.

X/Y-Plot
Erstellt ein Raster aus Bildern mit unterschiedlichen Parametern. Wählen Sie mithilfe von Feldern vom Typ X und Y aus, welche Parameter von Zeilen und Spalten gemeinsam genutzt werden sollen, und geben Sie diese Parameter durch Komma getrennt in die Felder X-Werte/Y-Werte ein. Für Integer- und Fließkommazahlen werden auch Bereiche unterstützt. Beispiele:

Einfache Bereiche:
1-5= 1, 2, 3, 4, 5
Bereiche mit Schrittweite in Klammern:
1-5 (+2)= 1, 3, 5
10-5 (-3)= 10, 7
1-3 (+0.5)= 1, 1,5, 2, 2,5, 3
Bereiche mit der Anzahl in eckigen Klammern:
1-10 [5]= 1, 3, 5, 7, 10
0.0-1.0 [6]= 0,0, 0,2, 0,4, 0,6, 0,8, 1,0


Hier sind die Einstellungen, die das obige Diagramm erstellen:



Aufforderung S/R
Prompt S/R ist eine der schwieriger zu verstehenden Betriebsarten für X/Y-Plot. S/R steht für Suchen/Ersetzen, und genau das tut es - Sie geben eine Liste von Wörtern oder Phrasen ein, es nimmt das erste aus der Liste und behandelt es als Schlüsselwort und ersetzt alle Instanzen dieses Schlüsselworts durch andere Einträge aus der Liste .

Beispielsweise erhalten Sie mit prompt a man holding an apple, 8k cleanund Prompt S/R an apple, a watermelon, a gundrei Eingabeaufforderungen:

a man holding an apple, 8k clean
a man holding a watermelon, 8k clean
a man holding a gun, 8k clean
Die Liste verwendet dieselbe Syntax wie eine Zeile in einer CSV-Datei. Wenn Sie also Kommas in Ihre Einträge einfügen möchten, müssen Sie Text in Anführungszeichen setzen und sicherstellen, dass zwischen Anführungszeichen und trennenden Kommas kein Leerzeichen steht:

darkness, light, green, heat- 4 Artikel - darkness, light, green,heat
darkness, "light, green", heat- FALSCH - 4 Artikel - darkness, "light, green",heat
darkness,"light, green",heat- RECHTS - 3 Artikel - darkness, light, green,heat
Textuelle Umkehrung
Kurze Erklärung: Legen Sie Ihre Einbettungen in das embeddingsVerzeichnis und verwenden Sie den Dateinamen in der Eingabeaufforderung.

Lange Erklärung: Textuelle Inversion

Gitter-0037

Größe ändern
Es gibt drei Optionen zum Ändern der Größe von Eingabebildern im img2img-Modus:

Nur Größe ändern - Ändert einfach die Größe des Quellbildes auf die Zielauflösung, was zu einem falschen Seitenverhältnis führt
Zuschneiden und Größe ändern - Ändern Sie die Größe des Quellbildes unter Beibehaltung des Seitenverhältnisses, sodass die gesamte Zielauflösung davon eingenommen wird, und schneiden Sie hervorstehende Teile ab
Größe ändern und füllen - Ändern Sie die Größe des Quellbildes unter Beibehaltung des Seitenverhältnisses, sodass es vollständig zur Zielauflösung passt, und füllen Sie den leeren Raum mit Zeilen/Spalten aus dem Quellbild
Beispiel: 

Auswahl der Probenahmemethode
Wählen Sie aus mehreren Sampling-Methoden für txt2img aus:



Seed-Größenänderung
Mit dieser Funktion können Sie Bilder von bekannten Samen mit unterschiedlichen Auflösungen erzeugen. Wenn Sie die Auflösung ändern, ändert sich normalerweise das Bild vollständig, auch wenn Sie alle anderen Parameter einschließlich des Seeds beibehalten. Mit der Seed-Größenänderung geben Sie die Auflösung des Originalbilds an, und das Modell wird sehr wahrscheinlich etwas erzeugen, das ihm sehr ähnlich sieht, selbst bei einer anderen Auflösung. Im Beispiel unten ist das Bild ganz links 512 x 512, und andere werden mit genau denselben Parametern, aber mit größerer vertikaler Auflösung produziert.

Die Info	Bild
Seed-Größenänderung nicht aktiviert	
Größe des Seeds von 512 x 512 geändert	
Ancestral Sampler sind darin etwas schlechter als die anderen.

Sie finden diese Funktion, indem Sie auf das Kontrollkästchen "Extra" neben dem Seed klicken.

Variationen
Mit einem Variationsstärke-Schieberegler und einem Variations-Startfeld können Sie angeben, wie stark das vorhandene Bild geändert werden soll, damit es wie ein anderes aussieht. Bei maximaler Stärke erhalten Sie Bilder mit dem Variationssamen, mindestens Bilder mit dem ursprünglichen Samen (außer bei Verwendung von Ahnensammlern).



Sie finden diese Funktion, indem Sie auf das Kontrollkästchen "Extra" neben dem Seed klicken.

Stile
Klicken Sie auf die Schaltfläche „Eingabeaufforderung als Stil speichern“, um Ihre aktuelle Eingabeaufforderung in die Datei „styles.csv“, die Datei mit einer Sammlung von Stilen, zu schreiben. Ein Dropdown-Feld rechts neben der Eingabeaufforderung ermöglicht es Ihnen, einen beliebigen Stil aus den zuvor gespeicherten auszuwählen und ihn automatisch an Ihre Eingabe anzuhängen. Um einen Stil zu löschen, löschen Sie ihn manuell aus der Datei styles.csv und starten Sie das Programm neu.

Wenn Sie die spezielle Zeichenfolge {prompt}in Ihrem Stil verwenden, wird alles, was sich derzeit in der Eingabeaufforderung befindet, an dieser Position ersetzt, anstatt den Stil an Ihre Eingabeaufforderung anzuhängen.

Negative Aufforderung
Ermöglicht es Ihnen, eine weitere Eingabeaufforderung für Dinge zu verwenden, die das Modell beim Generieren des Bildes vermeiden sollte. Dies funktioniert, indem anstelle einer leeren Zeichenfolge die negative Aufforderung zur bedingungslosen Konditionierung im Sampling-Prozess verwendet wird.

Erweiterte Erklärung: Negative Eingabeaufforderung

Original	Negativ: lila	Negativ: Tentakel
		
CLIP-Interrogator
Ursprünglich von: https://github.com/pharmapsychotic/clip-interrogator

Mit dem CLIP-Interrogator können Sie die Eingabeaufforderung von einem Bild abrufen. Die Eingabeaufforderung erlaubt es Ihnen nicht, genau dieses Bild zu reproduzieren (und manchmal wird es nicht einmal annähernd sein), aber es kann ein guter Anfang sein.



Wenn Sie den CLIP-Interrogator zum ersten Mal ausführen, werden einige Gigabyte an Modellen heruntergeladen.

Der CLIP-Interrogator besteht aus zwei Teilen: Einer ist ein BLIP-Modell, das aus dem Bild eine Textbeschreibung erstellt. Anderes ist ein CLIP-Modell, das einige für das Bild relevante Zeilen aus einer Liste auswählt. Standardmäßig gibt es nur eine Liste - eine Liste von Künstlern (von artists.csv). Sie können weitere Listen hinzufügen, indem Sie wie folgt vorgehen:

erstellen interrogateSie das Verzeichnis am selben Ort wie webui
Fügen Sie Textdateien mit einer relevanten Beschreibung in jeder Zeile ein
Ein Beispiel für zu verwendende Textdateien finden Sie unter https://github.com/pharmapsychotic/clip-interrogator/tree/main/clip_interrogator/data . Tatsächlich können Sie einfach Dateien von dort nehmen und sie verwenden - überspringen Sie einfach die Datei "artists.txt", da Sie bereits eine Liste von Künstlern haben artists.csv(oder verwenden Sie diese auch, wer wird Sie daran hindern). Jede Datei fügt der endgültigen Beschreibung eine Textzeile hinzu. Wenn Sie ".top3." zu Dateiname, zum Beispiel , flavors.top3.txtwerden die drei relevantesten Zeilen aus dieser Datei zur Eingabeaufforderung hinzugefügt (andere Nummern funktionieren auch).

Es gibt Einstellungen, die für diese Funktion relevant sind:

Interrogate: keep models in VRAM- Entladen Sie Interrogate-Modelle nicht aus dem Speicher, nachdem Sie sie verwendet haben. Für Benutzer mit viel VRAM.
Interrogate: use artists from artists.csvartists.csv- fügt beim Verhör den Künstler hinzu . Kann nützlich sein, wenn Sie Ihre Künstlerliste im interrogateVerzeichnis haben
Interrogate: num_beams for BLIP- Parameter, der beeinflusst, wie detailliert Beschreibungen aus dem BLIP-Modell sind (der erste Teil der generierten Eingabeaufforderung)
Interrogate: minimum description length- Mindestlänge für den Text des BLIP-Modells
Interrogate: maximum descripton length- maximale Länge für den Text des BLIP-Modells
Interrogate: maximum number of lines in text file- Der Befrager berücksichtigt nur so viele erste Zeilen in einer Datei. Auf 0 eingestellt, ist der Standardwert 1500, was etwa so viel ist, wie eine 4-GB-Videokarte verarbeiten kann.
Sofortige Bearbeitung
xy_grid-0022-646033397

Die sofortige Bearbeitung ermöglicht es Ihnen, mit dem Abtasten eines Bildes zu beginnen, aber in der Mitte zu etwas anderem zu wechseln. Die Basissyntax dafür lautet:

[from:to:when]
Wobei fromund towillkürliche Texte sind und wheneine Zahl ist, die definiert, wie spät im Abtastzyklus der Wechsel erfolgen soll. Je später es ist, desto weniger Kraft hat das Modell, um den toText anstelle von fromText zu zeichnen. Wenn wheneine Zahl zwischen 0 und 1 ist, ist dies ein Bruchteil der Anzahl der Schritte, nach denen gewechselt werden muss. Wenn es sich um eine Ganzzahl größer als Null handelt, ist dies nur der Schritt, nach dem der Wechsel vorgenommen wird.

Das Verschachteln einer Eingabeaufforderung in einer anderen funktioniert.

Zusätzlich:

[to:when]- fügt tonach einer festgelegten Anzahl von Schritten zur Eingabeaufforderung hinzu ( when)
[from::when]- wird fromnach einer festgelegten Anzahl von Schritten aus der Eingabeaufforderung entfernt ( when)
Beispiel: a [fantasy:cyberpunk:16] landscape

Zu Beginn zeichnet das Modell a fantasy landscape.
Nach Schritt 16 wechselt es zum Zeichnen a cyberpunk landscapeund macht dort weiter, wo es mit Fantasie aufgehört hat.
Hier ist ein komplexeres Beispiel mit mehreren Bearbeitungen: fantasy landscape with a [mountain:lake:0.25] and [an oak:a christmas tree:0.75][ in foreground::0.6][ in background:0.25] [shoddy:masterful:0.5](Sampler hat 100 Schritte)

am Anfang,fantasy landscape with a mountain and an oak in foreground shoddy
nach Schritt 25,fantasy landscape with a lake and an oak in foreground in background shoddy
nach Schritt 50,fantasy landscape with a lake and an oak in foreground in background masterful
nach Schritt 60,fantasy landscape with a lake and an oak in background masterful
nach Schritt 75,fantasy landscape with a lake and a christmas tree in background masterful
Das Bild oben wurde mit der Eingabeaufforderung erstellt:

„Offizielles Porträt eines lächelnden Generals aus dem Zweiten Weltkrieg, [männlich: weiblich: 0,99], fröhliches, glückliches, detailliertes Gesicht, 20. Jahrhundert, sehr detailliert, filmische Beleuchtung, digitale Kunstmalerei von Greg Rutkowski

Und die Zahl 0,99 wird durch das ersetzt, was Sie in den Spaltenbeschriftungen auf dem Bild sehen.

Die letzte Spalte im Bild ist [männlich:weiblich:0.0], was im Wesentlichen bedeutet, dass Sie das Modell bitten, von Anfang an eine Frau zu zeichnen, ohne mit einem männlichen General zu beginnen, und deshalb sieht es so anders aus als andere.

Abwechselnde Wörter
Bequeme Syntax zum Austauschen aller anderen Schritte.

[cow|horse] in a field
Bei Schritt 1 lautet die Eingabeaufforderung „Kuh auf einem Feld“. Schritt 2 ist "Pferd auf einem Feld". Schritt 3 ist „Kuh auf dem Feld“ und so weiter.

Abwechselnde Wörter

Siehe erweitertes Beispiel unten. Bei Schritt 8 geht die Kette zurück von „Mann“ zu „Kuh“.

[cow|cow|horse|man|siberian tiger|ox|man] in a field
Prompt Editing wurde erstmals von Doggettx in diesem myspace.com-Beitrag implementiert .

Mieten. Fix
Eine bequeme Option, um Ihr Bild teilweise mit einer niedrigeren Auflösung zu rendern, es hochzuskalieren und dann Details mit einer hohen Auflösung hinzuzufügen. Standardmäßig macht txt2img schreckliche Bilder bei sehr hohen Auflösungen, und das macht es möglich, die Verwendung der kleinen Bildkomposition zu vermeiden. Aktiviert durch Aktivieren des Kontrollkästchens "Hires. fix" auf der txt2img-Seite.

Ohne	Mit
00262-836728130	00261-836728130
00345-950170121	00341-950170121
Kleine Bilder werden mit der Auflösung gerendert, die Sie mit den Schiebereglern für Breite/Höhe festlegen. Die Abmessungen großer Bilder werden durch drei Schieberegler gesteuert: "Skalieren um"-Multiplikator (Hires Upscale), "Breite ändern auf" und/oder "Höhe ändern auf" (Hires-Größe ändern).

Wenn „Breite ändern auf“ und „Höhe ändern auf“ 0 sind, wird „Skalieren um“ verwendet.
Wenn „Breite ändern auf“ 0 ist, wird „Höhe ändern auf“ aus Breite und Höhe berechnet.
Wenn „Höhe ändern auf“ 0 ist, wird „Breite ändern auf“ aus Breite und Höhe berechnet.
Wenn sowohl „Breite ändern auf“ als auch „Höhe ändern auf“ ungleich Null sind, wird das Bild auf mindestens diese Abmessungen hochskaliert und einige Teile werden abgeschnitten.
Upscaler
In einem Dropdown-Menü können Sie die Art des Upscalers auswählen, der zum Ändern der Bildgröße verwendet werden soll. Zusätzlich zu allen Hochskalierern, die Ihnen auf der Registerkarte „Extras“ zur Verfügung stehen, gibt es eine Option zum Hochskalieren eines latenten Raumbilds, mit dem die stabile Diffusion intern arbeitet – für ein 3x512x512-RGB-Bild wäre seine latente Raumdarstellung 4x64x64. Um zu sehen, was jeder latente Raum-Upscaler tut, können Sie die Denoising-Stärke auf 0 und die Hires-Schritte auf 1 setzen - Sie erhalten eine sehr gute Annäherung an die stabile Diffusion, mit der Sie bei einem hochskalierten Bild arbeiten würden.

Nachfolgend finden Sie Beispiele dafür, wie verschiedene latente Upscale-Modi aussehen.

Original
00084-2395363541
Latent, Latent (antialiased)	Latent (bikubisch), Latent (bikubisch, Antialiasing)	Latent (am nächsten)
00071-2395363541	00073-2395363541	00077-2395363541
Antialiased-Variationen wurden von einem Mitwirkenden veröffentlicht und scheinen mit Nicht-Antialiasing identisch zu sein.

Zusammensetzbare Diffusion
Eine Methode, um die Kombination mehrerer Eingabeaufforderungen zu ermöglichen. Kombinieren Sie Eingabeaufforderungen mit einem großgeschriebenen UND

a cat AND a dog
Unterstützt Gewichtungen für Eingabeaufforderungen: a cat :1.2 AND a dog AND a penguin :2.2 Der Standardwert für die Gewichtung ist 1. Dies kann sehr nützlich sein, um mehrere Einbettungen mit Ihrem Ergebnis zu kombinieren:creature_embedding in the woods:0.7 AND arcane_embedding:0.5 AND glitch_embedding:0.2

Die Verwendung eines Werts unter 0,1 hat kaum einen Effekt. a cat AND a dog:0.03erzeugt im Grunde die gleiche Ausgabe wiea cat

Dies könnte praktisch sein, um fein abgestimmte rekursive Variationen zu generieren, indem Sie weitere Eingabeaufforderungen an Ihre Gesamtzahl anhängen.creature_embedding on log AND frog:0.13 AND yellow eyes:0.08

Unterbrechen
Drücken Sie die Unterbrechungstaste, um die aktuelle Verarbeitung zu stoppen.

4 GB Grafikkartenunterstützung
Optimierungen für GPUs mit niedrigem VRAM. Damit sollte es möglich sein, 512x512 Bilder auf Grafikkarten mit 4GB Speicher zu erzeugen.

--lowvramist eine Neuimplementierung einer Optimierungsidee von basujindal . Das Modell ist in Module unterteilt, und nur ein Modul wird im GPU-Speicher gehalten. Wenn ein anderes Modul ausgeführt werden muss, wird das vorherige aus dem GPU-Speicher entfernt. Die Art dieser Optimierung führt dazu, dass die Verarbeitung langsamer läuft – etwa zehnmal langsamer im Vergleich zum normalen Betrieb auf meiner RTX 3090.

--medvramist eine weitere Optimierung, die die VRAM-Nutzung erheblich reduzieren sollte, indem bedingtes und unbedingtes Denoising nicht im selben Batch verarbeitet werden.

Diese Optimierungsimplementierung erfordert keine Modifikation des ursprünglichen Stable Diffusion-Codes.

Wiederherstellung des Gesichts
Ermöglicht das Verbessern von Gesichtern in Bildern mit GFPGAN oder CodeFormer. Es gibt ein Kontrollkästchen in jeder Registerkarte, um die Gesichtswiederherstellung zu verwenden, und auch eine separate Registerkarte, mit der Sie nur die Gesichtswiederherstellung für jedes Bild verwenden können, mit einem Schieberegler, der steuert, wie sichtbar der Effekt ist. Sie können in den Einstellungen zwischen den beiden Methoden wählen.

Original	GFPGAN	CodeFormer
		
Speichern
Klicken Sie unter dem Ausgabeabschnitt auf die Schaltfläche Speichern, und die generierten Bilder werden in einem Verzeichnis gespeichert, das in den Einstellungen angegeben ist. Generierungsparameter werden an eine CSV-Datei im selben Verzeichnis angehängt.

Wird geladen
Die Ladegrafik von Gradio wirkt sich sehr negativ auf die Verarbeitungsgeschwindigkeit des neuronalen Netzes aus. Meine RTX 3090 macht Bilder ca. 10% schneller wenn der Reiter mit Gradio nicht aktiv ist. Standardmäßig blendet die Benutzeroberfläche jetzt die Ladefortschrittsanimation aus und ersetzt sie durch statischen „Laden...“-Text, der den gleichen Effekt erzielt. Verwenden Sie die --no-progressbar-hidingBefehlszeilenoption, um dies rückgängig zu machen und Ladeanimationen anzuzeigen.

Sofortige Validierung
Stable Diffusion hat eine Begrenzung für die Eingabetextlänge. Wenn Ihre Eingabeaufforderung zu lang ist, erhalten Sie im Textausgabefeld eine Warnung, die anzeigt, welche Teile Ihres Textes abgeschnitten und vom Modell ignoriert wurden.

Png-Informationen
Fügt PNG Informationen über Generierungsparameter als Textabschnitt hinzu. Sie können diese Informationen später mit jeder Software anzeigen, die das Anzeigen von PNG-Chunk-Informationen unterstützt, zum Beispiel: https://www.nayuki.io/page/png-file-chunk-inspector

Einstellungen
Eine Registerkarte mit Einstellungen ermöglicht es Ihnen, die Benutzeroberfläche zu verwenden, um mehr als die Hälfte der Parameter zu bearbeiten, die zuvor Befehlszeile waren. Die Einstellungen werden in der Datei config.js gespeichert. Einstellungen, die als Befehlszeilenoptionen verbleiben, sind diejenigen, die beim Start erforderlich sind.

Dateinamen-Format
Das Images filename patternFeld auf der Registerkarte Einstellungen ermöglicht die Anpassung von generierten txt2img- und img2img-Bilddateinamen. Dieses Muster definiert die Generierungsparameter, die Sie in Dateinamen aufnehmen möchten, und ihre Reihenfolge. Die unterstützten Tags sind:

[steps], [cfg], [prompt], [prompt_no_styles], [prompt_spaces], [width], [height], [styles], [sampler], [seed], [model_hash], [prompt_words], [date], [datetime], [job_timestamp].

Diese Liste wird sich jedoch mit neuen Ergänzungen weiterentwickeln. Sie können eine aktuelle Liste der unterstützten Tags abrufen, indem Sie in der Benutzeroberfläche mit der Maus über das Label „Bilddateinamenmuster“ fahren.

Beispiel für ein Muster:[seed]-[steps]-[cfg]-[sampler]-[prompt_spaces]

Hinweis zu "Eingabeaufforderungs"-Tags: [prompt]fügt Unterstriche zwischen den Eingabeaufforderungswörtern ein, während [prompt_spaces]die Eingabeaufforderung intakt bleibt (einfacheres erneutes Kopieren/Einfügen in die Benutzeroberfläche). [prompt_words]ist eine vereinfachte und bereinigte Version Ihrer Eingabeaufforderung, die bereits zum Generieren von Unterverzeichnisnamen verwendet wird, nur mit den Wörtern Ihrer Eingabeaufforderung (ohne Satzzeichen).

Wenn Sie dieses Feld leer lassen, wird das Standardmuster angewendet ( [seed]-[prompt_spaces]).

Bitte beachten Sie, dass die Tags tatsächlich innerhalb des Musters ersetzt werden. Das bedeutet, dass Sie diesem Muster auch Nicht-Tag-Wörter hinzufügen können, um Dateinamen noch deutlicher zu machen. Zum Beispiel:s=[seed],p=[prompt_spaces]

Benutzerskripte
Wenn das Programm mit --allow-codeOption gestartet wird, steht unten auf der Seite unter Skripte -> Benutzerdefinierter Code ein zusätzliches Texteingabefeld für Skriptcode zur Verfügung. Es erlaubt Ihnen, Python-Code einzugeben, der die Arbeit mit dem Bild erledigt.

Greifen Sie im Code mithilfe der Variablen auf Parameter der Webbenutzeroberfläche zu und stellen Sie mithilfe der Funktion pAusgaben für die Webbenutzeroberfläche bereit . display(images, seed, info)Alle Globals aus dem Skript sind ebenfalls zugänglich.

Ein einfaches Skript, das das Bild nur verarbeitet und normal ausgibt:

import modules.processing

processed = modules.processing.process_images(p)

print("Seed was: " + str(processed.seed))

display(processed.images, processed.seed, processed.info)
UI-Konfig
Sie können Parameter für UI-Elemente ändern:

Funkgruppen: Standardauswahl
Schieberegler: Standardwert, Min, Max, Schritt
Kontrollkästchen: aktivierter Zustand
Text- und Zahleneingaben: Standardwerte
Die Datei ist ui-config.json im webui-Verzeichnis und wird automatisch erstellt, wenn Sie beim Programmstart keine haben.

Kontrollkästchen, die normalerweise einen versteckten Abschnitt erweitern würden, tun dies zunächst nicht, wenn sie als UI-Konfigurationseinträge festgelegt werden.

Einige Einstellungen unterbrechen die Verarbeitung, wie z. B. Schritt, der für Breite und Höhe nicht durch 64 teilbar ist, und andere, wie das Ändern der Standardfunktion auf der Registerkarte img2img, können die Benutzeroberfläche unterbrechen. Ich habe nicht vor, diese in naher Zukunft anzusprechen.

ESRGAN
Es ist möglich, ESRGAN-Modelle auf der Registerkarte Extras sowie in SD Upscale zu verwenden.

Um ESRGAN-Modelle zu verwenden, legen Sie sie im ESRGAN-Verzeichnis am selben Speicherort wie webui.py ab. Eine Datei wird als Modell geladen, wenn sie die Erweiterung .pth hat. Holen Sie sich Modelle aus der Modelldatenbank .

Nicht alle Modelle aus der Datenbank werden unterstützt. Alle 2x-Modelle werden höchstwahrscheinlich nicht unterstützt.

img2img alternativer Test
Dekonstruiert ein Eingabebild unter Verwendung einer Umkehrung des Euler-Diffusors, um das Rauschmuster zu erstellen, das zum Erstellen der Eingabeaufforderung verwendet wird.

Als Beispiel können Sie dieses Bild verwenden. Wählen Sie den alternativen Test img2img aus dem Skriptabschnitt aus .

alt_src

Passen Sie Ihre Einstellungen für den Rekonstruktionsprozess an:

Verwenden Sie eine kurze Beschreibung der Szene: "Eine lächelnde Frau mit braunen Haaren." Es hilft, Funktionen zu beschreiben, die Sie ändern möchten. Legen Sie dies als Ihre Startaufforderung und „Ursprüngliche Eingabeaufforderung“ in den Skripteinstellungen fest.
Sie MÜSSEN die Euler-Stichprobenmethode verwenden, da dieses Skript darauf aufbaut.
Abtastschritte: 50-60. Dies VIEL stimmt mit dem Wert der Dekodierungsschritte im Skript überein, oder Sie werden eine schlechte Zeit haben. Verwenden Sie 50 für diese Demo.
CFG-Skala: 2 oder niedriger. Verwenden Sie für diese Demo 1.8. (Hinweis, Sie können ui-config.json bearbeiten, um "img2img/CFG Scale/step" auf .1 statt .5 zu ändern.
Denoising-Stärke - dies spielt eine Rolle, im Gegensatz zu dem, was die alten Dokumente sagten. Setzen Sie es auf 1.
Breite/Höhe – Verwenden Sie die Breite/Höhe des Eingabebilds.
Seed ... das kannst du ignorieren. Der umgekehrte Euler erzeugt jetzt das Rauschen für das Bild.
Decode cfg scale - Irgendwo unter 1 ist der Sweet Spot. Verwenden Sie für die Demo 1.
Dekodierungsschritte - wie oben erwähnt, sollte dies mit Ihren Sampling-Schritten übereinstimmen. 50 für die Demo, erwägen Sie eine Erhöhung auf 60 für detailliertere Bilder.
Sobald alle oben genannten Punkte eingegeben sind, sollten Sie in der Lage sein, auf "Generieren" zu klicken und ein Ergebnis zu erhalten, das dem Original sehr nahe kommt.

Nachdem Sie überprüft haben, dass das Skript das Quellfoto mit einem guten Maß an Genauigkeit neu generiert, können Sie versuchen, die Details der Eingabeaufforderung zu ändern. Größere Variationen des Originals führen wahrscheinlich zu einem Bild mit einer völlig anderen Zusammensetzung als die Quelle.

Beispielausgaben mit den obigen Einstellungen und Eingabeaufforderungen unten (Rotes Haar/Pony nicht abgebildet)

Demo

"Eine lächelnde Frau mit blauen Haaren." Funktioniert. "Eine stirnrunzelnde Frau mit braunen Haaren." Funktioniert. "Eine stirnrunzelnde Frau mit roten Haaren." Funktioniert. "Eine stirnrunzelnde Frau mit roten Haaren, die auf einem Pferd reitet." Scheint die Frau vollständig zu ersetzen, und jetzt haben wir ein rotes Pony.

user.css
Erstellen Sie eine Datei namens user.cssnear webui.pyund fügen Sie benutzerdefinierten CSS-Code ein. Dadurch wird die Galerie beispielsweise größer:

#txt2img_gallery, #img2img_gallery{
    min-height: 768px;
}
Ein nützlicher Tipp ist, dass Sie /?__theme=darkan Ihre Webui-URL anhängen können, um ein eingebautes dunkles Thema
zu aktivieren, z. ( http://127.0.0.1:7860/?__theme=dark)

Alternativ können Sie --theme=darkdas set COMMANDLINE_ARGS=in webui-user.bat
zB hinzufügenset COMMANDLINE_ARGS=--theme=dark

chrome_O1kvfKs1es

Benachrichtigung.mp3
Wenn eine benannte Audiodatei notification.mp3im Stammordner von webui vorhanden ist, wird sie abgespielt, wenn der Generierungsprozess abgeschlossen ist.

Als Inspirationsquelle:

https://pixabay.com/sound-effects/search/ding/?duration=0-30
https://pixabay.com/sound-effects/search/notification/?duration=0-30
Optimierungen
Ignoriere die letzten Schichten des CLIP-Modells
Dies ist ein Schieberegler in den Einstellungen und steuert, wie früh die Verarbeitung der Eingabeaufforderung durch das CLIP-Netzwerk gestoppt werden soll.

Eine genauere Erklärung:

CLIP ist ein sehr fortschrittliches neuronales Netzwerk, das Ihren Eingabeaufforderungstext in eine numerische Darstellung umwandelt. Neuronale Netze funktionieren sehr gut mit dieser numerischen Darstellung, und deshalb wählten Entwickler von SD CLIP als eines von drei Modellen, die an der Methode der stabilen Diffusion zur Erzeugung von Bildern beteiligt sind. Da CLIP ein neuronales Netzwerk ist, bedeutet dies, dass es viele Schichten hat. Ihre Eingabeaufforderung wird auf einfache Weise digitalisiert und dann durch Schichten geführt. Sie erhalten eine numerische Darstellung der Eingabeaufforderung nach der ersten Schicht, Sie speisen diese in die zweite Schicht ein, Sie speisen das Ergebnis davon in die dritte usw., bis Sie zur letzten Schicht gelangen, und das ist die Ausgabe von CLIP, die in Stable verwendet wird Diffusion. Dies ist der Schiebereglerwert von 1. Aber Sie können früher aufhören und die Ausgabe der vorletzten Ebene verwenden - das ist der Schiebereglerwert von 2. Je früher Sie aufhören,

Einige Modelle wurden mit dieser Art von Optimierung trainiert, sodass das Festlegen dieses Werts dazu beiträgt, bessere Ergebnisse bei diesen Modellen zu erzielen.

