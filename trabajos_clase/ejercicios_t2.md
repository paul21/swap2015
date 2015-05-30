Disponibilidad inicial
-----------------------

Web		85%
Application	90%
Database	99.9%
DNS		98%
Firewall	85%
Switch		99%
Data Center	99.99%
ISP		95%


Disponibilidad Web 2 = 0.85 + (1-0.85) * 0.85 = 0.9775 = 97.75%
Disponibilidad Web 3 = 0.9775 + (1-0.9775) * 0.85 = 0.996625 = 99.6625%

Disponibilidad App 2 = 0.9 + (1-0.9) * 0.9 = 0.99 = 99%
Disponibilidad App 3 = 0.99 + (1-0.99) * 0.9 = 0.999 = 99.9%

Disponibilidad DB 2  = 0.999 + (1-0.999) * 0.999 = 0.999999 = 99.9999%
Disponibilidad DB 3  = 0.999999 + (1-0.999999) * 0.999 = 0.999999999 = 99.999999%

Disponibilidad DNS 2 = 0.98 * (1-0.98) * 0.98 = 0.9996 = 99.96%
Disponibilidad DNS 3 = 0.9996 + (1-0.9996) * 0.98 = 0.999992 = 99.9992%

Disponibilidad Fw 2  = 0.85 + (1-0.85) * 0.85 = 0.9775 = 97.75%
Disponibilidad Fw 3  = 0.9775 + (1-0.9775) * 0.85 = 0.996625 = 99.6625%

Disponibilidad Sw 2  = 0.99 + (1-0.99) * 0.99 = 0.9999 = 99.99%
Disponibilidad Sw 3  = 0.9999 + (1-0.9999) * 0.99 = 0.999999 = 99.9999%

Disponibilidad DC 2  = 0.9999 + (1-0.9999) * 0.9999 = 0.99999999 = 99.999999%
Disponibilidad DC 3  = 0.99999999 + (1-0.99999999) * 0.9999 = 0.999999999999  = 99.999999999

Disponibilidad ISP 2 = 0.95 + (1-0.95) * 0.95 = 0.9975 = 99.75%
Disponibilidad ISP 3 = 0.9975 + (1-0.99975) * 0.95 = 0.999875 = 99.9875%


Disponibilidad final
--------------------

Web		99.6625%
Application	99.9%
Database	99.999999%
DNS		99.9992%
Firewall	99.6625%
Switch		99.9999%
Data Center	99.9999999999%
ISP		99.9875%


DISPONIBILIDAD DEL SISTEMA =  0.996625 * 0.999 * 0.999999999 * 0.999992 * 0.996625 * 0.999999 * 0.999999999999 * 0.999875 = 0.992135 = 99.2135%


**2.1 Buscar frameworks y librerías para diferentes lenguajes que permitan hacer aplicaciones altamente disponibles con relativa facilidad**
