1. Debemos instalar las siguientes herramientas:
	- *textlive-full*
	- *zathuta*
	- *latexmk*
	- *rubber*
2. Cambiamos la configuración con los siguientes comandos:
	```bash
xdg-mime query default application/pdf
xdg-mime default zathura.desktop application/pdf
	```
3. Instalamos *poppler-utils*, que permite convertir un pdf a texto.