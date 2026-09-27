# Anexo A — Proceso de extracción y análisis de IoAs

## Propósito

El propósito de este documento es mostrar el proceso de extracción de los IoAs (Indicadores de Ataque), el análisis realizado, las tablas generadas y los prompts usados para el desarrollo de la investigación.

## Contexto

El objetivo de este proceso es la extracción de Indicadores de ataque de documentos oficiales de diferentes proveedores de tecnología y servicios de ciberseguridad, con el fin de identificar cuáles son los más comunes. El proceso se basó en el documento oficial y el uso de la Inteligencia Artificial comercial Chat GPT para la extracción y mapeo de los indicadores, de acuerdo a la información que se presenta en los reportes de los proveedores. Para la verificación se realizó un proceso de dos pasos en primera se realiza una revisión manual de cada una de las tablas generadas y la comparación con la matriz de ataques y técnicas de MITRE, una vez limpiadas estas tablas, se procede a mediante un prompt para revisión RAG (Retrieval Augmente Generation) sobre las Tablas corregidas en el segundo análisis, con el fin eliminar las posibles alucinaciones que pudo tener la herramienta.

El objetivo de este proceso además del resultado para el análisis en el artículo, es hacer uso de recursos no especializados que permitan dar la base para procesos de análisis similar.

## Prompt para la generación inicial de las tablas

Este es el prompt usado para el proceso inicial de extracción y mapeo de los IoA que se puedan encontrar en el reporte, este también fue generado con apoyo de IA.

```text
Contexto
Estás analizando un reporte de inteligencia con el objetivo de identificar vectores
de ataque y mapearlos a técnicas de MITRE ATT&CK. Tu tarea es extraer
patrones de amenazas y vectores de ataque directamente del reporte, y
construir una tabla estructurada.

Objetivo
Generar una tabla con la siguiente estructura:
| Amenaza | Vector de Ataque | Descripción del comportamiento | Técnica MITRE asociada |

Reglas de extracción
1. Usar únicamente información del reporte proporcionado.
2. Identificar las amenazas o patrones de ataque mencionados en el reporte, por ejemplo:
   - Phishing
   - Credential abuse
   - Exploitation of vulnerabilities
   - Ransomware
   - Social engineering
3. Para cada amenaza, identificar el vector de ataque asociado descrito en el reporte.
4. Generar una descripción breve del comportamiento del ataque basada en el texto del reporte.
5. Mapear cada vector a la técnica correspondiente de MITRE ATT&CK.

Reglas para el mapeo a MITRE
- Si el reporte menciona claramente una sub-técnica, inclúyela.
- Si el reporte NO menciona una sub-técnica, usa solo la técnica principal de MITRE.
- No inventes sub-técnicas que no estén explícitamente justificadas por el reporte.

Ejemplo:
Correcto: T1566 – Phishing
Incorrecto (si el reporte no lo menciona): T1566.002 – Spearphishing Link

Restricciones importantes
- No usar información externa al reporte entregado.
- No inferir actores APT si el reporte no los menciona.
- No inventar vectores de ataque.
- La tabla debe estar basada únicamente en evidencia textual del reporte.

Formato de salida esperado
La respuesta debe generarse en formato de tabla:

Amenaza | Vector de Ataque | Descripción del comportamiento | Técnica MITRE asociada
Phishing | Email malicioso | Correos fraudulentos que inducen al usuario a revelar credenciales o ejecutar contenido malicioso | T1566 – Phishing

Criterios de calidad
La tabla debe:
- reflejar fielmente la información del reporte
- mantener consistencia entre amenaza y vector
- utilizar terminología clara
- evitar suposiciones no respaldadas por el texto

Resultado esperado
Una tabla estructurada que permita posteriormente realizar:

Amenaza
↓
Vector de ataque
↓
Técnica MITRE

Esto será utilizado para análisis de patrones de ataque y detección temprana basada en IoA.
```

Con este prompt se generan los diferentes espacios de trabajo con cada uno de los reportes.

Como resultado se genera la siguiente tabla en la que se representan las tácticas identificadas en cada reporte.

## Tabla 1 — Amenazas, vectores y técnicas identificadas por reporte

### Cisco Talos Year in Review (2024)

| Amenaza | Vectores | Técnica |
| --- | --- | --- |
| Explotación de vulnerabilidades | Public-facing applications / sistemas expuestos y vulnerables | T1190 - Exploit Public-Facing Application |
| Phishing | Malicious link | T1566 - Phishing |
| Phishing | Malicious attachment | T1566 - Phishing |
| Phishing | Vishing | T1566 - Phishing |
| Ransomware | Valid accounts | T1078 - Valid Accounts |
| Ransomware | Public-facing application | T1190 - Exploit Public-Facing Application |
| Ransomware | Drive-by compromise | T1189 - Drive-by Compromise |
| Ataques basados en identidad | Valid accounts | T1078 - Valid Accounts |
| Ataques basados en identidad | OS credential dumping | T1003 - OS Credential Dumping |
| Ataques basados en identidad | Brute force or password spray | T1110 - Brute Force |
| Ataques basados en identidad | Bypass MFA | T1111 - Multi-Factor Authentication Interception |
| Ataques basados en identidad | AiTM | T1557 - Adversary-in-the-Middle |
| Ataques basados en identidad | Web browser credentials | T1555.003 - Credentials from Web Browsers |
| Ataques basados en identidad | Kerberoasting | T1558.003 - Kerberoasting |
| Ataques basados en identidad | Pass the hash | T1550.002 - Pass the Hash |
| Ataques contra MFA | Password spray | T1110.003 - Password Spraying |
| Ataques contra MFA | Push spray / MFA bombing / fatigue | T1621 - Multi-Factor Authentication Request Generation |
| Ataques contra MFA | RDP brute force | T1110 - Brute Force |
| Ataques contra MFA | SSH brute force | T1110 - Brute Force |
| Ataques contra MFA | DDoS | T1498 - Network Denial of Service |

### Verizon Data Breach Investigations Report (2025)

| Amenaza | Vectores | Técnica |
| --- | --- | --- |
| System Intrusion | Credenciales comprometidas / explotación de vulnerabilidades / phishing | T1078 - Valid Accounts<br>T1190 - Exploit Public-Facing Application |
| Social Engineering | Phishing / Pretexting / Prompt bombing / Baiting | T1566 - Phishing<br>T1621 - Multi-Factor Authentication Request Generation |
| Basic Web Application Attacks | Uso de credenciales robadas / brute force / explotación de vulnerabilidades | T1078 - Valid Accounts<br>T1110 - Brute Force<br>T1190 - Exploit Public-Facing Application |
| Privilege Misuse | LAN access / Remote access con acceso legítimo | T1078 - Valid Accounts |
| Denial of Service | DDoS / tráfico distribuido desde múltiples puntos de Internet | T1498 - Network Denial of Service |

### Thales Data Threat Report (2026)

| Amenaza | Vectores | Técnica |
| --- | --- | --- |
| Desinformación / deepfakes con IA | Misinformation y deepfakes generados por IA | T1656 - Impersonation |
| Exposición de datos sensibles en IA/LLM | Ataques AI/LLM dirigidos a exponer datos sensibles | T1190 - Exploit Public-Facing Application |
| Explotación de vulnerabilidades conocidas | Exploitation of known vulnerability | T1190 - Exploit Public-Facing Application |
| Explotación de vulnerabilidades zero-day / novedosas | Exploitation of zero-day / novel / previously unknown vulnerability | T1190 - Exploit Public-Facing Application |
| Robo o compromiso de credenciales | Credential theft / compromise, including misappropriated secrets | T1078 - Valid Accounts |
| Explotación de componentes de terceros | Vulnerabilities from third parties, including external code and APIs | T1190 - Exploit Public-Facing Application |
| Inyección de malware | Malware injection | T1195 - Supply Chain Compromise |
| Compromiso de infraestructura | Infrastructure compromise | T1190 - Exploit Public-Facing Application |
| Abuso de controles de identidad y acceso | Exploit of identity/access controls for the cloud infrastructure environment | T1078 - Valid Accounts |
| Denegación de servicio | Denial of service | T1498 - Network Denial of Service |

### Google Secutiry Cloud Threat Horizon (2025)

| Amenaza | Vectores | Técnica |
| --- | --- | --- |
| Abuso de credenciales | Credenciales débiles o ausentes | T1078 - Valid Accounts |
| Explotación de fallas de configuración | Acceso por misconfiguración | T1190 - Exploit Public-Facing Application |
| Compromiso de interfaces expuestas | Compromiso de API/UI | T1190 - Exploit Public-Facing Application |
| Exposición de credenciales | Credenciales filtradas | T1078 - Valid Accounts |
| Explotación de vulnerabilidades | Remote Code Execution (RCE) | T1190 - Exploit Public-Facing Application |
| Sabotaje de recuperación / ransomware | Acceso a respaldos, borrado de rutinas y modificación de permisos | T1490 - Inhibit System Recovery<br>T1098 - Account Manipulation |
| Ingeniería social | Falsa oferta laboral y ejecución de contenedor Docker malicioso | T1566.002 - Spearphishing Link<br>T1566.003 - Spearphishing via Service<br>T1204.003 - Malicious Image |
| Secuestro de sesión y evasión MFA | Robo de credenciales/cookies y desactivación o bypass de MFA | T1539 - Steal Web Session Cookie<br>T1550.004 - Web Session Cookie<br>T1556.006 - Multi-Factor Authentication |
| Entrega de señuelos desde servicios confiables | Uso de Google Drive, SharePoint, Dropbox o GitHub para alojar PDFs señuelo | T1566 - Phishing<br>T1204 - User Execution |
| Dropper inicial en Linux | Archivos .desktop maliciosos con PDF benigno como distracción | T1204.002 - Malicious File<br>T1105 - Ingress Tool Transfer |
| Compromiso de cadena de suministro de extensiones | Cuenta de desarrollador comprometida / abuso de OAuth para publicar actualizaciones maliciosas | T1195 - Supply Chain Compromise |

### Crowdstrike Global Threat Report (2026)

| Amenaza | Vectores | Técnica |
| --- | --- | --- |
| Phishing potenciado por AI | Mensajes y páginas de phishing generadas con AI | T1566 - Phishing |
| ClickFix con AI | Lures ClickFix traducidos con AI | T1204 - User Execution |
| Fraude laboral con identidades falsas | Personas y cuentas falsas generadas con AI | T1656 - Impersonation |
| Malware LLM-enabled | Spear-phishing que distribuye LAMEHUG | T1566 - Phishing |
| Explotación de plataforma AI | Exploit de CVE-2025-3248 en Langflow | T1190 - Exploit Public-Facing Application |
| Vishing con acceso remoto | Voice phishing + Quick Assist/RMM | T1566.004 - Spearphishing Voice |
| Fake CAPTCHA | CAPTCHA falso que induce descarga y ejecución | T1204 - User Execution |
| Abuso del help desk | Reset de contraseña por ingeniería social | T1656 - Impersonation |
| Robo de credenciales desde VM no gestionada | Montaje de VMDK y extracción de NTDS | T1003 - OS Credential Dumping |
| Cifrado remoto por SMB | Ransomware desde host no gestionado contra shares SMB | T1486 - Data Encrypted for Impact |
| Ransomware cross-domain | Edge device sin parchear -> identidad cloud -> ESXi | T1190 - Exploit Public-Facing Application |
| Explotación China-nexus de perímetro | VPN appliances, firewalls, gateways e internet-facing systems | T1190 - Exploit Public-Facing Application |
| Pivot de edge device a vCenter | Cadena de exploits en VPN appliances | T1190 - Exploit Public-Facing Application |
| ShadowPad tras compromiso de VPN | DLL search-order hijacking y DNS tunneling | T1574 - Hijack Execution Flow |
| Weaponization acelerada de exploits | SQLi, file upload y React2Shell | T1190 - Exploit Public-Facing Application |
| Supply chain sobre proveedor de software | Proyecto Python troyanizado -> credenciales de dev -> modificación de frontend/smart contract | T1195 - Supply Chain Compromise |
| Mecanismo de actualización comprometido | Update legítimo de Notepad++ con payload malicioso | T1195 - Supply Chain Compromise |
| npm malicioso en falso proceso de selección | Paquetes npm maliciosos + señuelo de recruiter | T1195 - Supply Chain Compromise |
| Information stealer autopropagable | Paquete npm comprometido + GitHub pull requests maliciosos | T1195 - Supply Chain Compromise |
| Phishing a maintainer de npm | Página falsa de login de npm | T1566 - Phishing |
| Explotación de webmail | Zimbra/Roundcube con zero-days y XSS | T1190 - Exploit Public-Facing Application |
| Zero-days en VPN y edge servers | Explotación de dispositivos de red expuestos | T1190 - Exploit Public-Facing Application |
| Zero-days oportunistas en apps enterprise | Aplicaciones web empresariales expuestas a Internet | T1190 - Exploit Public-Facing Application |
| Escalamiento local con zero-day | CVE-2025-32706 en Windows CLFS | T1068 - Exploitation for Privilege Escalation |
| Abuso de identidad híbrida | Entra ID / Entra Connect Sync / AD FS / alt MFA / bypass de conditional access | T1098 - Account Manipulation |
| Abuso de relaciones de confianza B2B | Trusted relationship connections entre tenants de Entra ID | T1199 - Trusted Relationship |
| Phishing OAuth/device-code sobre infraestructura legítima | Enlaces OAuth 2.0 y device code hacia páginas reales de Microsoft | T1566 - Phishing |
| AiTM contra M365 y CRM | Kits AiTM, incluido EvilGinx2 | T1557 - Adversary-in-the-Middle |

### EINSA Threat Landscape (2025)

| Amenaza | Vectores | Técnica |
| --- | --- | --- |
| Phishing | Phishing | T1566 - Phishing |
| Explotación de vulnerabilidades | Vulnerabilidades explotadas rápidamente tras su divulgación | T1190 - Exploit Public-Facing Application |
| Botnets | Infraestructura botnet | T1584.005 - Botnet |
| Aplicaciones maliciosas / troyanizadas | Software o apps comprometidas | T1195 - Supply Chain Compromise |
| Acceso no autorizado por insiders | Uso indebido de acceso interno | T1078 - Valid Accounts |
| DDoS | Distributed Denial of Service | T1498 - Network Denial of Service |
| Defacement | Alteración de sitios web | T1491 - Defacement |
| Ransomware | Cifrado/extorsión | T1486 - Data Encrypted for Impact |
| Banking trojan | Malware bancario | T1657 - Financial Theft |
| Infostealer | Robo de información local | T1005 - Data from Local System |
| Credential theft | Robo de credenciales | T1555 - Credentials from Password Stores |
| Strategic data collection | Recolección / exfiltración de datos estratégicos | T1005 - Data from Local System |
| Riesgo de cadena de suministro | Compromiso de dependencias y terceros | T1195 - Supply Chain Compromise |
| ClickFix | CAPTCHA falso que induce ejecución de PowerShell | T1059.001 - PowerShell |
| Drive-by download en WordPress comprometido | Sitios WordPress comprometidos | T1189 - Drive-by Compromise |
| PhaaS | Kits de phishing automatizados | T1566 - Phishing |
| Phishing por iMessage / RCS | Mensajería móvil | T1566 - Phishing |
| Adversary-in-the-Middl e phishing | Portal falso que intercepta autenticación | T1557 - Adversary-in-the-Middle |
| Quishing | Códigos QR maliciosos en PDF | T1566 - Phishing |
| Compromiso de terceros / proveedores | Brecha en proveedor externo | T1195 - Supply Chain Compromise |
| npm malicioso en GitHub | Paquetes Node falsos | T1195 - Supply Chain Compromise |
| Extensiones de navegador maliciosas | Browser extensions comprometidas | T1176.001 - Browser Extensions |
| Fraude bancario ODF / ATO (Medusa) | On-Device Fraud / Account Takeover | T1657 - Financial Theft |
| Explotación de vulnerabilidad Qualcomm DSP | Vulnerabilidad en chipsets móviles | T1190 - Exploit Public-Facing Application |
| Perfiles falsos / job lures | LinkedIn y solicitudes de empleo fabricadas | T1566 - Phishing |
| Phishing asistido por IA | Correos de phishing generados o mejorados con LLMs | T1566 - Phishing |
| Sitios / instaladores falsos de herramientas de IA | Suplantación de herramientas de IA populares | T1189 - Drive-by Compromise |
| Compromiso de la cadena de suministro de IA | Modelos ML, PyPI, rules files, slopsquatting | T1195 - Supply Chain Compromise |

### Entel Reporte de Ciberseguridad (2026)

| Amenaza | Vectores | Técnica |
| --- | --- | --- |
| Ransomware | Accesos iniciales vendidos por IABs | T1078 - Valid Accounts |
| Ransomware | Ingeniería social / phishing | T1566 - Phishing |
| Akira | VPN sin MFA y credenciales comprometidas | T1078 - Valid Accounts |
| Ransomware / Data breach | Explotación de servicios expuestos y aplicaciones públicas | T1190 - Exploit Public-Facing Application |
| Data breach | Automatización maliciosa orientada a exponer datos sensibles | T1213 - Data from Information Repositories |
| Data breach en Chile | Venta de accesos WP Admin / accesos comprometidos | T1078 - Valid Accounts |
| Infostealer | Phishing / spam | T1566 - Phishing |
| Infostealer | Malvertising, sitios clonados y descargas engañosas | T1189 - Drive-by Compromise |
| Infostealer | Instaladores falsos, software crackeado, loaders y exploit kits | T1204 - User Execution |
| Infostealer | Robo de credenciales, cookies, tokens MFA y claves API | T1555 - Credentials from Password Stores |
| Infostealer | Exfiltración a servidores remotos, Telegram y servicios cloud | T1041 - Exfiltration Over C2 Channel |
| Infostealer | Ejecución en memoria, bypass UAC y cambios de registro | T1548 - Abuse Elevation Control Mechanism |
| Abuso de APIs | Fallas de validación, enforcement y lógica de negocio | T1190 - Exploit Public-Facing Application |
| Abuso de APIs | Scraping, bots y fraude automatizado | T1213 - Data from Information Repositories |
| Abuso de APIs | Fuerza bruta y validación encubierta de credenciales | T1110 - Brute Force |
| Abuso de APIs | Rate limiting insuficiente / interrupción de servicio | T1499 - Endpoint Denial of Service |
| Riesgo cloud | IAM mal configurado y mala gestión de credenciales | T1078 - Valid Accounts |
| IA en cloud | Modelos de IA expuestos vía API / abuso de endpoints / prompt injection | T1190 - Exploit Public-Facing Application |
| IA/ML en cloud | Manipulación de vector DB y pipelines MLOps | T1565 - Data Manipulation |
| APT contra infraestructura crítica | Explotación de appliances, VPN, borde y compromiso de proveedores/cloud/MSPs | T1190 - Exploit Public-Facing Application<br>T1199 - Trusted Relationship |
| APT contra infraestructura crítica | LoTL, robo de credenciales, túneles, reconocimiento OT y exfiltración/preposicionamiento | T1218 - System Binary Proxy Execution<br>T1003 - OS Credential Dumping<br>T1090 - Proxy<br>T1046 - Network Service Discovery<br>T1567.002 - Exfiltration to Cloud Storage |
| Qilin | Credenciales válidas y explotación de servicios vulnerables | T1078 - Valid Accounts<br>T1190 - Exploit Public-Facing Application |
| Qilin | Persistencia, ejecución y evasión | T1053 - Scheduled Task<br>T1136 - Create Account<br>T1059 - Command and Scripting Interpreter<br>T1027 - Obfuscated Files or Information |
| Qilin | Robo de credenciales y movimiento lateral | T1003 - OS Credential Dumping<br>T1021 - Remote Services |
| Qilin | Exfiltración previa, cifrado y eliminación de respaldos | T1041 - Exfiltration Over C2 Channel<br>T1486 - Data Encrypted for Impact<br>T1490 - Inhibit System Recovery |

### Microsoft Digital Defense Report (2025)

| Amenaza | Vectores | Técnica |
| --- | --- | --- |
| ClickFix | Copiado y pegado de comandos maliciosos | T1204 - User Execution |
| Phishing | Correo o mensaje fraudulento | T1566 - Phishing |
| Password spray | Intentos distribuidos de autenticación | T1110 - Brute Force |
| Drive-by compromise / SEO poisoning | Sitios o resultados manipulados | T1189 - Drive-by Compromise |
| Explotación de vulnerabilidades | Aplicaciones expuestas a Internet | T1190 - Exploit Public-Facing Application |
| Servicios remotos expuestos | Acceso por servicios remotos externos | T1133 - External Remote Services |
| Compromiso de cadena de suministro | Relación de confianza con terceros | T1195 - Supply Chain Compromise |
| Abuso de cuentas válidas | Uso de credenciales robadas | T1078 - Valid Accounts |
| Falso soporte TI | Email bombing + vishing + Teams impersonation | T1566 - Phishing |
| Malvertising | Anuncios engañosos con malware | T1189 - Drive-by Compromise |
| Infostealer | Malware para robo de credenciales | T1056 - Input Capture |
| App consent phishing | Consentimiento OAuth malicioso | T1098 - Account Manipulation |
| Abuso de identidades cloud | Aplicaciones OAuth maliciosas | T1098 - Account Manipulation |
| Device code phishing | Flujo de autenticación con código de dispositivo | T1566 - Phishing |
| AiTM | Interposición en autenticación | T1557 - Adversary-in-the-Middle |
| Robo de tokens | Sustracción de token autenticado | T1528 - Steal Application Access Token |
| Intercepción de código de un solo uso | OTC intercept | T1111 - Multi-Factor Authentication Interception |
| Compromiso del sistema de autenticación | Robo de signing key | T1649 - Steal or Forge Authentication Certificates |
| BEC | Compromiso de identidad y correo corporativo | T1078 - Valid Accounts |
| Exfiltración de correo | Acceso y extracción desde buzones | T1114 - Email Collection |
| Deepfake impersonation | Audio/video sintético para fraude | T1566 - Phishing |
| Identidades sintéticas | Creación de identidades falsas | T1585 - Establish Accounts |
| Perfiles falsos en LinkedIn | Suplantación como reclutador o proveedor | T1585 - Establish Accounts |
| Impersonación de dominios | Creación masiva de dominios similares | T1583 - Acquire Infrastructure |
| Access brokers | Venta de acceso inicial basado en credenciales | T1078 - Valid Accounts |
| Access brokers | Explotación de vulnerabilidades | T1190 - Exploit Public-Facing Application |
| RDP como acceso inicial | Herramientas RDP | T1133 - External Remote Services |
| Portales corporativos remotos | Portales de acceso remoto | T1133 - External Remote Services |
| RMM para intrusión | Herramientas de administración remota | T1219 - Remote Access Software |
| Ransomware | Cifrado con impacto | T1486 - Data Encrypted for Impact |
| Evasión de AV | Abuso de exclusiones antivirus | T1562 - Impair Defenses |
| Exfiltración de datos | Robo masivo de información | T1041 - Exfiltration Over C2 Channel |
| Ataques destructivos en cloud | Borrado masivo / acciones destructivas | T1485 - Data Destruction |
| Robo de credenciales y access keys cloud | Sustracción de secretos de acceso | T1552 - Unsecured Credentials |
| RCE en Azure | Uso de Azure Run Command | T1059 - Command and Scripting Interpreter |
| Cryptojacking en contenedores | Secuestro de recursos | T1496 - Resource Hijacking |
| Web shell en contenedores | Persistencia en componente servidor | T1505 - Server Software Component |
| Trabajadores TI norcoreanos | Infiltración como insider | T1078 - Valid Accounts |
| Data poisoning | Manipulación de datos de entrenamiento | T1565 - Data Manipulation |

### Mandiant M -Thrends (2025)

| Amenaza | Vectores | Técnica |
| --- | --- | --- |
| Explotación de vulnerabilidades | Exploit de vulnerabilidad | T1190 - Exploit Public-Facing Application |
| Abuso de credenciales robadas | Credenciales filtradas o robadas | T1078 - Valid Accounts |
| Phishing por correo | Email phishing | T1566 - Phishing |
| Compromiso web | Drive-by, malvertising, SEO poisoning, sitios comprometidos | T1189 - Drive-by Compromise |
| Compromiso previo | Acceso ya comprometido y luego reutilizado/revendido | T1078 - Valid Accounts |
| Fuerza bruta | Password spraying / intentos masivos / abuso de RDP o VPN | T1110 - Brute Force |
| Amenaza interna fraudulenta | Falsos empleados / DPRK IT workers | T1078 - Valid Accounts |
| Compromiso de terceros | Third-party compromise | T1199 - Trusted Relationship |
| Vishing | Llamadas de ingeniería social por voz | T1566.004 - Spearphishing Voice |
| Toma de cuentas por SIM swapping | SIM swapping | T1078 - Valid Accounts |
| Compromiso de cadena de suministro | Supply chain compromise | T1195 - Supply Chain Compromise |
| Infección por medios removibles / BYOD | BYOD con USB infectado | T1091 - Replication Through Removable Media |
| Ransomware / extorsión | Fuerza bruta contra infraestructura remota | T1110 - Brute Force |
| Infostealer malware | Logs de infostealer, cookies y credenciales robadas | T1539 - Steal Web Session Cookie |
| Compromiso cloud por phishing | Email phishing en entornos cloud | T1566 - Phishing |
| Compromiso cloud por credenciales | Stolen credentials en cloud | T1078 - Valid Accounts |
| Abuso de mesa de ayuda / SSO | Llamadas al help desk para resetear password y MFA | T1566.004 - Spearphishing Voice |
| Exfiltración en cloud/SaaS | Utilidades de sincronización hacia almacenamiento del atacante | T1567.002 - Exfiltration to Cloud Storage |
| Spear-phishing iraní | Lures de entrenamiento/webinar con enlaces en servicios de archivos | T1566 - Phishing |
| Malware iraní disfrazado / wiper | Falsos instaladores o alertas de seguridad | T1204 - User Execution |
| Crypto drainer | Phishing + ingeniería social + smart contracts maliciosos | T1566 - Phishing |
| EtherHiding / malware vía Web3 | WordPress vulnerable + código que recupera payload desde smart contract | T1190 - Exploit Public-Facing Application |
| Robo desde repositorios inseguros | Repositorios con datos sensibles insuficientemente protegidos | T1213 - Data from Information Repositories |
| Repositorios inseguros usados para reconocimiento y pivoteo | Documentación interna, diagramas, playbooks y secretos mal almacenados | T1552 - Unsecured Credentials |

### Fortinet Threat Landscape Report (2025)

| Amenaza | Vectores | Técnica |
| --- | --- | --- |
| Reconocimiento automatizado | Escaneo activo global | T1595 - Active Scanning |
| Reconocimiento sobre VoIP | Escaneo SIP/VoIP | T1595 - Active Scanning |
| Reconocimiento OT/ICS | Escaneo Modbus TCP | T1595 - Active Scanning |
| Reconocimiento con herramientas ofensivas | SIPVicious, Qualys, Nmap, Nessus y OpenVAS | T1595 - Active Scanning |
| Abuso de credenciales filtradas | Combo lists | T1110.004 - Credential Stuffing |
| Acceso inicial comprado | Credenciales VPN corporativas vendidas por IABs | T1133 - External Remote Services |
| Acceso remoto comprado | Acceso RDP vendido por IABs | T1021.001 - Remote Desktop Protocol |
| Persistencia web preposicionada | Webshells vendidas en mercados clandestinos | T1505.003 - Server Software Component: Web Shell |
| Phishing asistido por IA | FraudGPT y WormGPT | T1566 - Phishing |
| Robo de credenciales MFA | EvilProxy y Robin Banks con capacidades AiTM | T1557 - Adversary-in-the-Middle |
| Vishing asistido por IA | ElevenLabs y Voicemy.ai | T1566 - Phishing |
| Explotación de servicios SMB | CVE-2017-0147 | T1210 - Exploitation of Remote Services |
| RCE en servicios expuestos | CVE-2021-44228 (Apache Log4j) | T1190 - Exploit Public-Facing Application |
| Abuso de contraseña embebida | CVE-2019-18935 en Netcore Netis | T1078 - Valid Accounts |
| Abuso de credenciales por defecto en IoT | Default credentials | T1078 - Valid Accounts |
| Explotación de cámaras expuestas | CVE-2017-18377 en GoAhead Cameras | T1190 - Exploit Public-Facing Application |
| Explotación de firewalls y routers | CVE-2022-30525 en Zyxel | T1190 - Exploit Public-Facing Application |
| Explotación de routers SOHO | CVE-2023-1389 en TP-Link Archer AX21 | T1190 - Exploit Public-Facing Application |
| Explotación de routers GPON | CVE-2018-10561 | T1190 - Exploit Public-Facing Application |
| Propagación lateral por SMB | Descarga de ejecutables maliciosos dentro de tráfico SMB | T1570 - Lateral Tool Transfer |
| Ejecución remota por WMI | WMI ExecMethod | T1047 - Windows Management Instrumentation |
| Movimiento lateral por RDP | RDP-based lateral movement | T1021.001 - Remote Desktop Protocol |
| Transferencia de malware | Malicious PE downloaded across networks | T1105 - Ingress Tool Transfer |
| Ejecución fileless en Windows | WMI-based execution of encoded PowerShell commands | T1059.001 - PowerShell |
| Manipulación de Active Directory | DCShadow | T1207 - Rogue Domain Controller |
| Robo de secretos de dominio | DCSync | T1003.006 - DCSync |
| Reconocimiento de dominio | Active Directory Enumeration | T1087 - Account Discovery |
| Enumeración de recursos compartidos | Network scanning de sesiones y shared resources | T1135 - Network Share Discovery |
| C2 cifrado | SSL C2 beacons | T1071 - Application Layer Protocol |
| C2 por DNS | Cobalt Strike DNS requests, DNS tunneling y long DNS queries | T1071.004 - Application Layer Protocol: DNS |
| Infraestructura C2 dinámica | DGA domains | T1568.002 - Domain Generation Algorithms |
| Compromiso de identidad cloud | New logins from unusual locations | T1078 - Valid Accounts |
| Acceso inicial cloud por engaño | Phishing exploits | T1566 - Phishing |
| Abuso de APIs cloud | New API activity for existing users | T1556.004 - Cloud Instance Metadata API Exploitation |
| Credenciales cloud expuestas | Credential leaks in code repositories | T1078 - Valid Accounts |
| Ejecución en cargas cloud comprometidas | Bash, PowerShell y Python | T1059 - Command and Scripting Interpreter |
| Persistencia y C2 en cloud | Abuso de cloud-hosted applications legítimas | T1102 - Web Service |
| Secuestro de recursos cloud | Cryptojacking | T1496 - Resource Hijacking |
| Misconfiguración cloud explotada | Open storage buckets y over-permissioned identities | T1190 - Exploit Public-Facing Application |

### Palo Alto Global Incident Response Report (2026)

| Amenaza | Vectores | Técnica |
| --- | --- | --- |
| Explotación acelerada de vulnerabilidades | Explotación rápida de CVE en activos expuestos | T1190 - Exploit Public-Facing Application |
| Ingeniería social hiperpersonalizada | Lures construidos con OSINT | T1566 - Phishing |
| Identidades sintéticas / deepfakes | Uso de identidades falsas para robar credenciales o pasar procesos de contratación | T1585 - Establish Accounts |
| Desarrollo malicioso asistido por IA | Generación de scripts maliciosos con LLM | T1587 - Develop Capabilities |
| Abuso de plataformas de IA empresariales | Uso de credenciales válidas para manipular IA corporativa o asistentes internos | T1078 - Valid Accounts |
| Phishing basado en identidad | Correos o señuelos orientados a credenciales | T1566 - Phishing |
| Ingeniería social para eludir MFA | Circunvención de MFA y secuestro de sesión | T1550 - Use Alternate Authentication Material |
| Credenciales previamente comprometidas | Reutilización de credenciales filtradas o compradas | T1078 - Valid Accounts |
| Fuerza bruta | Intentos repetidos de autenticación | T1110 - Brute Force |
| Misconfiguraciones de IAM | Abuso de políticas sobrepermisivas | T1078 - Valid Accounts |
| Amenaza interna | Abuso de credenciales legítimas por insiders | T1078 - Valid Accounts |
| Escalamiento por permisos excesivos | Roles sobredimensionados, grants heredados y privilegios no retirados | T1098 - Account Manipulation |
| Abuso de tokens y OAuth | Uso de tokens de sesión o grants ilícitos | T1550 - Use Alternate Authentication Material |
| Compromiso vía navegador | SEO poisoning, sitios falsos y herramientas adulteradas | T1189 - Drive-by Compromise |
| Compromiso de flujos de desarrollo cloud | Exposición o robo de credenciales cloud en repositorios y herramientas de depuración | T1552 - Unsecured Credentials |
| Compromiso de integraciones SaaS | Tokens OAuth válidos e integraciones con permisos heredados | T1550 - Use Alternate Authentication Material |
| Abuso de canales de gestión de proveedores | RMM y MDM usados como vía de operación | T1219 - Remote Access Tools |
| Paquetes maliciosos en dependencias | Compromiso en instalación o build | T1195 - Supply Chain Compromise |
| Aplicación heredada vulnerable de tercero | Interfaz no documentada, sin autenticación, con fallas estructurales | T1190 - Exploit Public-Facing Application |
| Explotación de infraestructura web por actor estatal | Web servers y bases de datos como objetivo directo | T1190 - Exploit Public-Facing Application |
| Persistencia en virtualización con C2 encubierto | BRICKSTORM ocultando tráfico C2 en sesiones web cifradas | T1071 - Application Layer Protocol |
| Infiltración laboral norcoreana | Empleo remoto fraudulento y acceso como contratista/empleado | T1585 - Establish Accounts |
| Entrevistas laborales maliciosas | Coding challenges y ejecución de código no verificado | T1204 - User Execution |
| Lures de reclutamiento iraníes | Portales falsos, email, LinkedIn y documentos de candidatura infectados | T1566 - Phishing |


Una vez generadas se realizó el uso de Excel para generar una matriz con todas las técnicas y tácticas identificadas identificadas y se comparó con la matriz de MITRE MITATT&CK realizando un conteo de en cuantos reportes exista dicha técnica y táctica. Como resultado se genera la tabla con la cual se realiza el análisis.

## Tabla 2 — Número de reportes donde se identifica la técnica y táctica asociada

| Técnica | Táctica | # de Reportes |
| --- | --- | --- |
| T1003 - OS Credential Dumping | TA0006 - Credential Access | 3 |
| T1003.006 - DCSync | TA0006 - Credential Access | 1 |
| T1005 - Data from Local System | TA0009 - Collection | 1 |
| T1021 - Remote Services | TA0008 - Lateral Movement | 1 |
| T1021.001 - Remote Desktop Protocol | TA0008 - Lateral Movement | 1 |
| T1027 - Obfuscated Files or Information | TA0005 - Defense Evansion | 1 |
| T1041 - Exfiltration Over C2 Channel | TA0010 - Exfiltration | 2 |
| T1046 - Network Service Discovery | TA0007 - Discovery | 1 |
| T1047 - Windows Management Instrumentation | TA0002 - Excecution | 1 |
| T1053 - Scheduled Task | TA0002 - Excecution | 1 |
| T1056 - Input Capture | TA0006 - Credential Access | 1 |
| T1059 - Command and Scripting Interpreter | TA0002 - Excecution | 3 |
| T1059.001 - PowerShell | TA0002 - Excecution | 2 |
| T1068 - Exploitation for Privilege Escalation | TA0004 - Privilege Escalation | 1 |
| T1071 - Application Layer Protocol | TA0011 - Command and Control | 2 |
| T1071.004 - Application Layer Protocol: DNS | TA0011 - Command and Control | 1 |
| T1078 - Valid Accounts | TA0001 - Initial Access | 10 |
| T1078 - Valid Accounts | TA0003 - Persistance | 10 |
| T1078 - Valid Accounts | TA0004 - Privilege Escalation | 10 |
| T1078 - Valid Accounts | TA0005 - Defense Evansion | 10 |
| T1087 - Account Discovery | TA0007 - Discovery | 1 |
| T1090 - Proxy | TA0011 - Command and Control | 1 |
| T1091 - Replication Through Removable Media | TA0001 - Initial Access | 1 |
| T1091 - Replication Through Removable Media | TA0008 - Lateral Movement | 1 |
| T1098 - Account Manipulation | TA0003 - Persistance | 4 |
| T1098 - Account Manipulation | TA0004 - Privilege Escalation | 4 |
| T1102 - Web Service | TA0011 - Command and Control | 1 |
| T1105 - Ingress Tool Transfer | TA0011 - Command and Control | 2 |
| T1110 - Brute Force | TA0006 - Credential Access | 6 |
| T1110.003 - Password Spraying | TA0006 - Credential Access | 1 |
| T1110.004 - Credential Stuffing | TA0006 - Credential Access | 1 |
| T1111 - Multi-Factor Authentication Interception | TA0006 - Credential Access | 2 |
| T1114 - Email Collection | TA0009 - Collection | 1 |
| T1133 - External Remote Services | TA0001 - Initial Access | 2 |
| T1133 - External Remote Services | TA0003 - Persistance | 2 |
| T1135 - Network Share Discovery | TA0007 - Discovery | 1 |
| T1136 - Create Account | TA0003 - Persistance | 1 |
| T1176.0001 - Browser Extensions | TA0003 - Persistance | 1 |
| T1189 - Drive-by Compromise | TA0001 - Initial Access | 6 |
| T1190 - Exploit Public-Facing Application | TA0001 - Initial Access | 11 |
| T1195 - Supply Chain Compromise | TA0001 - Initial Access | 7 |
| T1199 - Trusted Relationship | TA0001 - Initial Access | 3 |
| T1204 - User Execution | TA0002 - Excecution | 6 |
| T1204.002 - Malicious File | TA0002 - Excecution | 1 |
| T1204.003 - Malicious Image | TA0002 - Excecution | 1 |
| T1207 - Rogue Domain Controller | TA0005 - Defense Evansion | 1 |
| T1210 - Exploitation of Remote Services | TA0008 - Lateral Movement | 1 |
| T1213 - Data from Information Repositories | TA0009 - Collection | 2 |
| T1218 - System Binary Proxy Execution | TA0005 - Defense Evansion | 1 |
| T1219 - Remote Access Tools | TA0011 - Command and Control | 1 |
| T1485 - Data Destruction | TA0040 - Impact | 1 |
| T1486 - Data Encrypted for Impact | TA0040 - Impact | 4 |
| T1490 - Inhibit System Recovery | TA0040 - Impact | 2 |
| T1491 - Defacement | TA0040 - Impact | 1 |
| T1496 - Resource Hijacking | TA0040 - Impact | 2 |
| T1498 - Network Denial of Service | TA0040 - Impact | 4 |
| T1499 - Endpoint Denial of Service | TA0040 - Impact | 1 |
| T1505 - Server Software Component | TA0003 - Persistance | 1 |
| T1505.003 - Server Software Component: Web Shell | TA0003 - Persistance | 1 |
| T1528 - Steal Application Access Token | TA0006 - Credential Access | 1 |
| T1539 - Steal Web Session Cookie | TA0006 - Credential Access | 2 |
| T1548 - Abuse Elevation Control Mechanism | TA0004 - Privilege Escalation | 1 |
| T1548 - Abuse Elevation Control Mechanism | TA0005 - Defense Evansion | 1 |
| T1550.002 - Pass the Hash | TA0005 - Defense Evansion | 1 |
| T1550.004 - Web Session Cookie | TA0005 - Defense Evansion | 1 |
| T1552 - Unsecured Credentials | TA0006 - Credential Access | 3 |
| T1555 - Credentials from Password Stores | TA0006 - Credential Access | 1 |
| T1555.003 - Credentials from Web Browsers | TA0006 - Credential Access | 2 |
| T1556.004 - Cloud Instance Metadata API Exploitation | TA0006 - Credential Access | 1 |
| T1556.006 - Multi-Factor Authentication | TA0006 - Credential Access | 1 |
| T1557 - Adversary-in-the-Middle | TA0006 - Credential Access | 5 |
| T1557 - Adversary-in-the-Middle | TA0009 - Collection | 5 |
| T1558.003 - Kerberoasting | TA0006 - Credential Access | 1 |
| T1562 - Impair Defenses | TA0005 - Defense Evansion | 1 |
| T1565 - Data Manipulation | TA0040 - Impact | 2 |
| T1566 - Phishing | TA0001 - Initial Access | 10 |
| T1566.002 - Spearphishing Link | TA0001 - Initial Access | 1 |
| T1566.003 - Spearphishing via Service | TA0001 - Initial Access | 1 |
| T1566.004 - Spearphishing Voice | TA0001 - Initial Access | 2 |
| T1567.002 - Exfiltration to Cloud Storage | TA0010 - Exfiltration | 2 |
| T1568.002 - Domain Generation Algorithms | TA0011 - Command and Control | 1 |
| T1570 - Lateral Tool Transfer | TA0008 - Lateral Movement | 1 |
| T1574 - Hijack Execution Flow | TA0003 - Persistance | 1 |
| T1574 - Hijack Execution Flow | TA0004 - Privilege Escalation | 1 |
| T1574 - Hijack Execution Flow | TA0005 - Defense Evansion | 1 |
| T1583 - Acquire Infrastructure | TA0042 - Resource Development | 1 |
| T1584.005 - Botnet | TA0042 - Resource Development | 1 |
| T1585 - Establish Accounts | TA0042 - Resource Development | 2 |
| T1595 - Active Scanning | TA0043 - Reconnaissance | 1 |
| T1621 - Multi-Factor Authentication Request Generation | TA0006 - Credential Access | 2 |
| T1649 - Steal or Forge Authentication Certificates | TA0006 - Credential Access | 1 |
| T1656 - Impersonation | TA0005 - Defense Evansion | 2 |
| T1657 - Financial Theft | TA0040 - Impact | 1 |

## Segundo proceso de verificación

Como parte del proceso de verificación se genera el prompt RAG.

```text
Actúa como un agente RAG de verificación académica y anti-alucinaciones.

INSUMOS:
1. Documento fuente: Reporte original
2. Tabla a verificar con columnas:
   - Reporte
   - Año
   - Amenaza
   - Vectores
   - Técnica MITRE

OBJETIVO:
Verificar si cada fila de la tabla está respaldada por el reporte y si el mapeo
MITRE es metodológicamente válido.

IMPORTANTE:
El reporte usa principalmente la taxonomía Proveedor, no MITRE ATT&CK. Por
eso, no debes exigir que el código MITRE aparezca literalmente en el reporte.

Debes validar en dos capas:

CAPA A — VALIDACIÓN DOCUMENTAL Reporte
Para cada fila verifica:
1. ¿La amenaza o patrón aparece en el Reporte?
2. ¿El vector aparece en el Reporte?
3. ¿El DBIR relaciona ese vector con esa amenaza/patrón?
4. ¿La descripción del comportamiento coincide con lo que el reporte dice?

CAPA B — VALIDACIÓN MITRE
Para cada vector verifica:
1. ¿La técnica MITRE asignada representa correctamente el comportamiento?
2. ¿La técnica es demasiado general o demasiado específica?
3. Si el reporte no menciona subtécnicas, no valides subtécnicas como “directamente soportadas”.
4. Si el mapeo MITRE es una inferencia razonable desde el comportamiento,
   clasifícalo como “Inferencia fuerte” o “Inferencia media”, no como evidencia directa.

REGLAS:
- Usa únicamente el reporte original como fuente documental.
- No inventes vectores.
- No inventes relaciones amenaza-vector.
- No inventes grupos APT ni campañas.
- No agregues técnicas MITRE que no puedan justificarse desde el comportamiento.
- Si un vector no aparece claramente en el DBIR, márcalo como no soportado.
- Si la técnica MITRE no aparece en el DBIR pero corresponde al comportamiento,
  márcala como inferencia, no como evidencia directa.
- Si una fila contiene varios vectores o varias técnicas, divídela internamente y
  valida cada combinación vector-técnica por separado.

ESCALA DE VALIDACIÓN:
Usa estos estados:

✅ Confirmado:
La amenaza, el vector y su relación están claramente respaldados por el DBIR.

⚠ Parcialmente soportado:
La amenaza aparece, pero el vector o la relación no están completamente claros.

❌ No soportado:
No hay evidencia suficiente en el DBIR o parece información inventada.

NIVEL DEL MAPEO MITRE:
Usa una de estas categorías:

- Directamente soportado:
  El reporte menciona explícitamente el comportamiento y la técnica MITRE es equivalente directa.

- Inferencia fuerte:
  El reporte describe claramente el comportamiento, aunque no use MITRE.

- Inferencia media:
  El comportamiento aparece, pero el mapeo MITRE requiere interpretación.

- Inferencia débil:
  Hay poca evidencia o el vector es ambiguo.

- No soportado:
  La técnica MITRE no corresponde al comportamiento o el vector no aparece.

SCORE:
Asigna un puntaje de 0 a 100:
100 = evidencia textual exacta y relación clara.
75 = evidencia clara, aunque el mapeo MITRE es inferido.
50 = evidencia parcial o relación débil.
25 = inferencia débil con poca evidencia.
0 = no soportado o posible alucinación.

PROCEDIMIENTO:
Para cada fila:
1. Extrae la amenaza.
2. Separa cada vector individual.
3. Separa cada técnica MITRE individual.
4. Relaciona cada vector con su técnica correspondiente.
5. Busca evidencia en el Reporte original.
6. Cita página o sección del reporte.
7. Extrae fragmento textual breve.
8. Evalúa la relación amenaza-vector.
9. Evalúa el mapeo MITRE.
10. Asigna estado, nivel MITRE y score.
11. Propón corrección si aplica.

FORMATO DE SALIDA:

Tabla 1 — Verificación por combinación vector-técnica

| Fila original | Amenaza / Patrón DBIR | Vector evaluado | Técnica MITRE evaluada | Evidencia DBIR | Página / sección | Estado documental | Nivel MITRE | Score | Comentario |
|---|---|---|---|---|---|---|---|---|---|

Tabla 2 — Problemas detectados

| Tipo de problema | Fila afectada | Elemento problemático | Explicación | Corrección sugerida |
|---|---|---|---|---|

Tabla 3 — Versión corregida sugerida

| Reporte | Año | Amenaza | Vector de Ataque | Técnica MITRE asociada | Nivel de soporte |
|---|---|---|---|---|---|

CRITERIOS ESPECIALES PARA ESTA TABLA:

1. Para “System Intrusion”:
   Verifica si DBIR relaciona este patrón con ransomware, uso de credenciales,
   explotación de vulnerabilidades o phishing.

2. Para “Social Engineering”:
   Verifica individualmente:
   - Phishing
   - Pretexting
   - Prompt bombing
   - Baiting

3. Para “Basic Web Application Attacks”:
   Verifica individualmente:
   - uso de credenciales robadas
   - brute force
   - explotación de vulnerabilidades

4. Para “Privilege Misuse”:
   Verifica si “LAN access” y “Remote access con acceso legítimo” aparecen en el
   reporte y si T1078 es el mejor mapeo.

5. Para “Denial of Service”:
   Verifica si el reporte describe DDoS o tráfico distribuido desde múltiples puntos
   de Internet, y si T1498 es adecuado.

RESULTADO ESPERADO:
Un informe de validación que indique qué partes de la tabla están correctamente
respaldadas, cuáles son inferencias razonables y cuáles podrían ser alucinaciones.
```

Con este prompt se generan los diferentes espacios de trabajo con cada uno de los reportes.

## Tablas por proveedor

El resultado de este proceso de verificación son 2 tablas, una es la usada en los procesos de análisis posteriores, y la otra es la usada como segunda verificación de la información de la tabla usada para el análisis. Se recalca que las tablas de los proveedores se encuentran en el Anexo B y las de verificación se encuentran en el Anexo C.

El resultado de esto es una tabla más condensada en la que basado en el método de un voto por reporte de la técnica y táctica con el fin de remover posibles inclinaciones propias de un proveedor asociadas al servicio específico que proveen. Como resultado la escala de las técnicas y tácticas más comunes es de 1 a 11. Lo cual permite dar este resultado.

## Tabla 3 — Número de reportes donde se identifica la técnica y táctica (subtácticas) asociada

| Técnica | Táctica | # de Reportes |
| --- | --- | --- |
| T1190 - Exploit Public-Facing Application | TA0001 - Initial Access | 11 |
| T1078 - Valid Accounts | TA0001 - Initial Access / TA0003 - Persistance / TA0004 - Privilege Escalation / TA0005 - Defense Evansion | 10 |
| T1566 - Phishing | TA0001 - Initial Access | 10 |
| T1195 - Supply Chain Compromise | TA0001 - Initial Access | 7 |
| T1110 - Brute Force | TA0006 - Credential Access | 6 |
| T1189 - Drive-by Compromise | TA0001 - Initial Access | 6 |
| T1204 - User Execution | TA0002 - Excecution | 6 |
| T1557 - Adversary-in-the-Middle | TA0006 - Credential Access / TA0009 - Collection | 5 |
| T1098 - Account Manipulation | TA0003 - Persistance / TA0004 - Privilege Escalation | 4 |
| T1486 - Data Encrypted for Impact | TA0040 - Impact | 4 |
| T1498 - Network Denial of Service | TA0040 - Impact | 4 |
| T1003 - OS Credential Dumping | TA0006 - Credential Access | 3 |
| T1059 - Command and Scripting Interpreter | TA0002 - Excecution | 3 |
| T1199 - Trusted Relationship | TA0001 - Initial Access | 3 |
| T1552 - Unsecured Credentials | TA0006 - Credential Access | 3 |
| T1041 - Exfiltration Over C2 Channel | TA0010 - Exfiltration | 2 |
| T1059.001 - PowerShell | TA0002 - Excecution | 2 |
| T1071 - Application Layer Protocol | TA0011 - Command and Control | 2 |
| T1105 - Ingress Tool Transfer | TA0011 - Command and Control | 2 |
| T1111 - Multi-Factor Authentication Interception | TA0006 - Credential Access | 2 |
| T1133 - External Remote Services | TA0001 - Initial Access / TA0003 - Persistance | 2 |
| T1213 - Data from Information Repositories | TA0009 - Collection | 2 |
| T1490 - Inhibit System Recovery | TA0040 - Impact | 2 |
| T1496 - Resource Hijacking | TA0040 - Impact | 2 |
| T1539 - Steal Web Session Cookie | TA0006 - Credential Access | 2 |
| T1555.003 - Credentials from Web Browsers | TA0006 - Credential Access | 2 |
| T1565 - Data Manipulation | TA0040 - Impact | 2 |
| T1566.004 - Spearphishing Voice | TA0001 - Initial Access | 2 |
| T1567.002 - Exfiltration to Cloud Storage | TA0010 - Exfiltration | 2 |
| T1585 - Establish Accounts | TA0042 - Resource Development | 2 |
| T1621 - Multi-Factor Authentication Request Generation | TA0006 - Credential Access | 2 |
| T1656 - Impersonation | TA0005 - Defense Evansion | 2 |
| T1003.006 - DCSync | TA0006 - Credential Access | 1 |
| T1005 - Data from Local System | TA0009 - Collection | 1 |
| T1021 - Remote Services | TA0008 - Lateral Movement | 1 |
| T1021.001 - Remote Desktop Protocol | TA0008 - Lateral Movement | 1 |
| T1027 - Obfuscated Files or Information | TA0005 - Defense Evansion | 1 |
| T1046 - Network Service Discovery | TA0007 - Discovery | 1 |
| T1047 - Windows Management Instrumentation | TA0002 - Excecution | 1 |
| T1053 - Scheduled Task | TA0002 - Excecution | 1 |
| T1056 - Input Capture | TA0006 - Credential Access | 1 |
| T1068 - Exploitation for Privilege Escalation | TA0004 - Privilege Escalation | 1 |
| T1071.004 - Application Layer Protocol: DNS | TA0011 - Command and Control | 1 |
| T1087 - Account Discovery | TA0007 - Discovery | 1 |
| T1090 - Proxy | TA0011 - Command and Control | 1 |
| T1091 - Replication Through Removable Media | TA0001 - Initial Access / TA0008 - Lateral Movement | 1 |
| T1102 - Web Service | TA0011 - Command and Control | 1 |
| T1110.003 - Password Spraying | TA0006 - Credential Access | 1 |
| T1110.004 - Credential Stuffing | TA0006 - Credential Access | 1 |
| T1114 - Email Collection | TA0009 - Collection | 1 |
| T1135 - Network Share Discovery | TA0007 - Discovery | 1 |
| T1136 - Create Account | TA0003 - Persistance | 1 |
| T1176.0001 - Browser Extensions | TA0003 - Persistance | 1 |
| T1204.002 - Malicious File | TA0002 - Excecution | 1 |
| T1204.003 - Malicious Image | TA0002 - Excecution | 1 |
| T1207 - Rogue Domain Controller | TA0005 - Defense Evansion | 1 |
| T1210 - Exploitation of Remote Services | TA0008 - Lateral Movement | 1 |
| T1218 - System Binary Proxy Execution | TA0005 - Defense Evansion | 1 |
| T1219 - Remote Access Tools | TA0011 - Command and Control | 1 |
| T1485 - Data Destruction | TA0040 - Impact | 1 |
| T1491 - Defacement | TA0040 - Impact | 1 |
| T1499 - Endpoint Denial of Service | TA0040 - Impact | 1 |
| T1505 - Server Software Component | TA0003 - Persistance | 1 |
| T1505.003 - Server Software Component: Web Shell | TA0003 - Persistance | 1 |
| T1528 - Steal Application Access Token | TA0006 - Credential Access | 1 |
| T1548 - Abuse Elevation Control Mechanism | TA0004 - Privilege Escalation / TA0005 - Defense Evansion | 1 |
| T1550.002 - Pass the Hash | TA0005 - Defense Evansion | 1 |
| T1550.004 - Web Session Cookie | TA0005 - Defense Evansion | 1 |
| T1555 - Credentials from Password Stores | TA0006 - Credential Access | 1 |
| T1556.004 - Cloud Instance Metadata API Exploitation | TA0006 - Credential Access | 1 |
| T1556.006 - Multi-Factor Authentication | TA0006 - Credential Access | 1 |
| T1558.003 - Kerberoasting | TA0006 - Credential Access | 1 |
| T1562 - Impair Defenses | TA0005 - Defense Evansion | 1 |
| T1566.002 - Spearphishing Link | TA0001 - Initial Access | 1 |
| T1566.003 - Spearphishing via Service | TA0001 - Initial Access | 1 |
| T1568.002 - Domain Generation Algorithms | TA0011 - Command and Control | 1 |
| T1570 - Lateral Tool Transfer | TA0008 - Lateral Movement | 1 |
| T1574 - Hijack Execution Flow | TA0003 - Persistance / TA0004 - Privilege Escalation / TA0005 - Defense Evansion | 1 |
| T1583 - Acquire Infrastructure | TA0042 - Resource Development | 1 |
| T1584.005 - Botnet | TA0042 - Resource Development | 1 |
| T1595 - Active Scanning | TA0043 - Reconnaissance | 1 |
| T1649 - Steal or Forge Authentication Certificates | TA0006 - Credential Access | 1 |
| T1657 - Financial Theft | TA0040 - Impact | 1 |

Basado en este análisis se genera el producto final de este proceso que es la generación de posibles IoAs para usar como elementos en un posible log asociado a este comportamiento.

## Tabla 4 — Técnica, IoA, eventos observables y campos del log identificados

| Técnica | IoA | Eventos observables | Campo requerido en el Log |
| --- | --- | --- | --- |
| T1190 - Exploit Public-Facing Application | Envío de solicitudes especialmente construidas a una aplicación expuesta a Internet, seguidas de errores anómalos del servicio y comportamiento pos-explotación en el host o contenedor. (MITRE ATT&CK) | solicitud HTTP/S o de servicio con patrones anómalos; aumento de respuestas 4xx/5xx o reinicios/crash del servicio; creación de proceso hijo desde el servicio web o intérprete; escritura de webshell o módulo no habitual; conexión saliente nueva desde el servidor comprometido | timestamp; hostname; service_name; process_name; parent_process; command_line; source_ip; source_port; destination_ip; destination_port; http_method; url_path; user_agent; status_code; event_id; file_path; file_hash; module_name; protocol; result (MITRE ATT&CK) |
| T1078 - Valid Accounts | Uso de credenciales legítimas desde orígenes, horarios, geografías o tipos de inicio de sesión atípicos, seguido de actividad impropia para la cuenta o cuenta de servicio. (MITRE ATT&CK) | autenticación exitosa desde IP o ubicación inusual; inicio de sesión interactivo o remoto por cuenta de servicio; uso anómalo de sudo/su o privilegios; múltiples intentos MFA o fallos MFA asociados; acceso a IdP, VPN, OWA o RDP fuera del patrón esperado | timestamp; username; user_id; account_type; hostname; source_ip; source_port; destination_ip; application/service_name; logon_type; authentication_result; mfa_result; geo_location; session_id; process_name; parent_process; privilege_level; event_id (MITRE ATT&CK) |
| T1566 - Phishing | Recepción de mensajes de phishing con enlaces o adjuntos que desencadenan apertura, descarga, ejecución o actividad de autenticación anómala posterior al mensaje. (MITRE ATT&CK) | recepción de correo o mensaje con adjunto o URL; apertura del mensaje por el usuario; guardado de archivo adjunto o clic sobre enlace; creación de proceso nuevo desde cliente de correo, navegador u Office; conexión saliente o autenticación anómala posterior al mensaje | timestamp; recipient_user; sender_address; sender_domain; message_id; subject; attachment_name; attachment_hash; embedded_url; email_auth_result; client_app; hostname; process_name; parent_process; file_path; destination_ip; destination_domain; authentication_result; mfa_result (MITRE ATT&CK) |
| T1566.002 - Spearphishing Link | Correo dirigido con enlace malicioso que provoca navegación a dominios sospechosos, descarga de contenido y ejecución de procesos inusuales o concesión anómala de consentimiento OAuth. (MITRE ATT&CK) | recepción de email con URL incrustada; clic del usuario sobre el enlace; navegación del navegador a dominio nuevo, ofuscado o lookalike; descarga de archivo o contenido desde el sitio visitado; creación de procesos hijo desde el navegador; evento de consentimiento OAuth o concesión de token inusual | timestamp; recipient_user; sender_address; sender_domain; message_id; subject; embedded_url; clicked_url; referrer; browser_name; hostname; process_name; parent_process; command_line; file_name; file_path; destination_domain; destination_ip; oauth_app_id; consent_result; token_id (MITRE ATT&CK) |
| T1566.003 - Spearphishing via Service | Mensajes dirigidos enviados por servicios de terceros o no corporativos que derivan en descarga de archivos, escritura inesperada en disco y ejecución descendiente desde navegador o aplicación de productividad. (MITRE ATT&CK) | acceso o uso de Gmail, LinkedIn, Teams, Telegram u otro servicio externo; recepción de mensaje con enlace o archivo; descarga o escritura inesperada de archivo desde navegador o app; ejecución de proceso hijo desde navegador, Mail.app o app de productividad; intercambio de archivo o enlace seguido de actividad de red anómala | timestamp; username; external_service_name; account_id; sender_identifier; message_id; url; attachment_name; file_path; file_hash; hostname; browser_name; process_name; parent_process; command_line; destination_domain; destination_ip; action; result (MITRE ATT&CK) |
| T1566.004 - Spearphishing Voice | Llamadas o comunicaciones de voz que inducen al usuario a visitar URL, descargar herramientas o instalar software de acceso remoto, seguidas de actividad sospechosa en el equipo o MFA. (MITRE ATT&CK) | registro de llamada entrante o saliente con número inusual; navegación web posterior a la llamada; descarga de script, instalador o herramienta RMM; ejecución o instalación de software de acceso remoto tras la llamada; eventos de MFA push fatigue o consentimientos anómalos correlacionados con la llamada | timestamp; user_id; device_id; caller_number; callee_number; call_direction; call_duration; voip_session_id; hostname; url; file_name; file_path; process_name; parent_process; command_line; software_name; installation_result; mfa_result; destination_ip; destination_domain (MITRE ATT&CK) |
| T1195 - Supply Chain Compromise | Instalación o actualización de software, dependencias o imágenes desde fuentes atípicas o con discrepancias de firma/hash, seguida de reemplazo de binarios y comportamiento anómalo en la primera ejecución. (MITRE ATT&CK) | instalación o actualización desde repositorio o fuente no aprobada; discrepancia de firma digital o hash; escritura de binarios en rutas inesperadas o reemplazo de archivos firmados; carga de módulos no firmados o anómalos en la primera ejecución; conexión saliente a destinos nuevos después de instalar o actualizar | timestamp; hostname; username; package_name; package_version; repository_url; update_source; installer_name; file_path; file_hash; signature_status; signer; process_name; parent_process; child_process; module_name; destination_ip; destination_domain; event_id; result (MITRE ATT&CK) |
| T1110 - Brute Force | Múltiples intentos repetitivos de autenticación sobre una o varias cuentas, seguidos eventualmente de éxito desde la misma fuente o en una ventana temporal sospechosa. (MITRE ATT&CK) | alto volumen de autenticaciones fallidas; intentos repetidos contra una o varias cuentas; autenticación exitosa posterior a múltiples fallos; intentos sobre servicios remotos como SSH, RDP, VPN, OWA o SaaS; bloqueos de cuenta o alertas por umbral de autenticación | timestamp; username; source_ip; source_port; destination_ip; destination_port; hostname; service_name; protocol; logon_type; authentication_result; failure_reason; account_lockout_status; session_id; event_id (MITRE ATT&CK) |
| T1110.003 - Password Spraying | Uso de una misma contraseña, o un conjunto muy reducido, contra muchas cuentas distintas en un intervalo definido para evitar bloqueos por cuenta. (MITRE ATT&CK) | fallos de autenticación en múltiples cuentas con mismo origen; repetición de patrón de contraseña común sobre muchos usuarios; intentos contra SSH, Kerberos, LDAP, OWA, SSO o SaaS; baja frecuencia por cuenta pero amplitud sobre muchas identidades; fallos distribuidos desde misma IP o firma de cliente | timestamp; source_ip; source_port; destination_ip; service_name; protocol; username; authentication_result; failure_reason; attempted_password_pattern; client_app; hostname; event_id; time_window_id (MITRE ATT&CK) |
| T1110.004 - Credential Stuffing | Uso de pares usuario-contraseña filtrados de servicios ajenos para autenticar contra múltiples identidades desde una misma fuente o sesión, con ráfagas de fallos y posibles bloqueos. (MITRE ATT&CK) | múltiples fallos con pares distintos usuario-contraseña desde una sola IP; ráfagas de autenticación sobre distintos usuarios en SSH, RDP, OWA, Azure AD, Okta u otros; reutilización de credenciales filtradas contra identidades diversas; bloqueos de cuenta o elevación del conteo de fallos; éxito eventual sobre una cuenta después de múltiples pruebas | timestamp; source_ip; source_port; destination_ip; service_name; protocol; username; authentication_result; failure_reason; credential_pair_id o attempted_username; session_id; hostname; client_app; account_lockout_status; event_id (MITRE ATT&CK) |
| T1189 - Drive-by Compromise | Navegación legítima a un sitio comprometido o malvertising que desencadena carga de recursos web sospechosos, inyección de scripts y posterior ejecución, inyección o caída de artefactos en el endpoint. (MITRE ATT&CK) | solicitud a dominio o recurso no habitual desde el navegador; carga de JavaScript u otros recursos ofuscados o mutados; creación de proceso hijo o intérprete desde el navegador; escritura de archivos temporales o artefactos en disco; modificación de memoria, carga de módulo o cambio de registro tras la navegación; conexión saliente tipo C2 posterior | timestamp; username; hostname; browser_name; process_name; parent_process; command_line; url; referrer; destination_domain; destination_ip; http_method; status_code; file_path; file_hash; module_name; registry_key; dns_query; event_id (MITRE ATT&CK) |
| T1204 - User Execution | Apertura o clic por parte del usuario sobre contenido malicioso que produce creación o extracción de archivos y ejecución de binarios, instaladores o LOLBins con tráfico saliente inmediato. (MITRE ATT&CK) | apertura de documento, enlace o archivo desde app de usuario; creación o extracción de archivo en rutas escribibles por el usuario; ejecución de powershell, cmd, mshta, rundll32, bash, python u otro cargador desde la app padre; instalación de software o herramienta remota; conexión HTTP(S), DNS o SMB desde la misma cadena de procesos | timestamp; username; hostname; application_name; process_name; parent_process; command_line; file_name; file_path; file_hash; file_origin; destination_ip; destination_domain; protocol; session_id; event_id; result (MITRE ATT&CK) |
| T1204.002 - Malicious File | Apertura de archivo descargado, compartido o adjunto que termina en escritura en rutas de usuario y ejecución de utilidades sospechosas o cargadores del sistema. (MITRE ATT&CK) | apertura de documento, instalador, ISO, ZIP o script por el usuario; creación de archivo en Downloads, Temp, Desktop o medio removible; extracción o descompresión previa a la ejecución; proceso hijo sospechoso desde Word, PDF reader, archiver o app similar; evento de cuarentena o marca de archivo descargado; conexión saliente posterior a la apertura | timestamp; username; hostname; application_name; process_name; parent_process; command_line; file_name; file_path; file_hash; file_origin_url; file_origin_host; quarantine_status; integrity_level; destination_ip; destination_domain; event_id (MITRE ATT&CK) |
| T1204.003 - Malicious Image | Descarga y despliegue por el usuario de una imagen de contenedor o VM no confiable, seguida de inicio de instancia/contenedor y ejecución de utilidades o conexiones no esperadas en el arranque. (MITRE ATT&CK) | descarga o pull de imagen desde registro público o no aprobado; creación de contenedor o instancia desde imagen nueva o no vista; inicio de contenedor o VM; ejecución de curl, wget, bash, python, cloud-init u otra utilidad al primer arranque; conexión saliente anómala desde el namespace, contenedor o instancia recién creada | timestamp; user_id; service_account; image_name; image_tag; image_digest; image_registry; image_source; container_id; container_name; instance_id; cloud_account_id; project_or_subscription; namespace; process_name; command_line; destination_ip; destination_domain; action; result (MITRE ATT&CK) |


# Proceso de generacion de Log
