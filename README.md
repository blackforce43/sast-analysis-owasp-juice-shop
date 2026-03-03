# Security analysis of [OWASP Juice Shop](https://github.com/juice-shop/juice-shop) application

## Application summary

OWASP Juice Shop - небезопасное веб-приложение, созданное организацией OWASP (Open Web Application Security Project) для обучения безопасности, демонстрации работы уязвимостей (в частности, но не ограничиваясь, из категории [OWASP Top 10](https://owasp.org/Top10/2021/)), CTF-соревнований и тестирования инструментов безопасности (SAST/OSA/SCA/DAST).

### Installation through Docker Container
1. Установить [Docker](https://www.docker.com/)
2. Выполнить команду ```docker pull bkimminich/juice-shop```
3. Выполнить команду ```docker run --rm -p 127.0.0.1:3000:3000 bkimminich/juice-shop```
4. В браузере перейти по url [http://localhost:3000](http://localhost:3000)

## SASTs used

| Инструмент | Назначение |
| - | - |
| Bearer | Анализ кода и потоков данных |
| TruffleHog | Поиск секретов/утечек |
| Semgrep | Классический анализ кода |

### Bearer

#### Installation and use

1. Выполнить команду ```curl -sfL https://raw.githubusercontent.com/Bearer/bearer/main/contrib/install.sh | sh```
2. Перейти в директорию с приложением и выполнить команду ```bearer scan juice-shop```

#### Security Audit

Просканировано 396 файлов, найдена 341 потенциальная уязвимость:
- CRITICAL: 21 (CWE-502, CWE-798, CWE-89, CWE-95)
- HIGH: 20 (CWE-22, CWE-312, CWE-73, CWE-79, CWE-918)
- MEDIUM: 34 (CWE-1333, CWE-208, CWE-328, CWE-532, CWE-601, CWE-693)
- LOW: 266 (CWE-330, CWE-532, CWE-548)
- WARNING: 0

Результаты сканирования содержатся в отдельном файле [```results/bearer.txt```](https://github.com/blackforce43/sast-analysis-owasp-juice-shop/blob/main/results/bearer.txt)

### TruffleHog

#### Installation and use

1. Скачать и распаковать нужный архив с ```https://github.com/trufflesecurity/trufflehog/releases/latest```
2. Переместить бинарник в PATH ```sudo mv trufflehog /usr/local/bin/```
4. Перейти в директорию приложения и выполнить команду ```trufflehog filesystem .```

#### Security Audit

Ознакомиться с результатами сканирования (проблемы с секретами и конфиденциальными данными) можно в отдельном файле [```results/trufflehog.txt```](https://github.com/blackforce43/sast-analysis-owasp-juice-shop/blob/main/results/trufflehog.txt)

### Semgrep

#### Installation and use

1. Установить python3 и pip ```sudo apt install python3-pip```
2. Установить semgrep ```python3 -m pip install semgrep```
3. Перейти в директорию приложения и выполнить команду ```semgrep --config=auto .```

#### Security Audit

В результате скнирования была найдена 41 потенциальная уязвимость. Полный результат сканирования содержится в отдельном файле [```results/semgrep.txt```](https://github.com/blackforce43/sast-analysis-owasp-juice-shop/blob/main/results/semgrep.txt)

## Conclusion

Для оценки работы инструментов статического анализа уязвимостей на примере веб-приложения Juice Shop, были проанализированы результаты сканирования на предмет False Positive и отобраны по 3 различные находки с подробным анализом. Полный отчёт находится в файле [```report/report.md```](https://github.com/blackforce43/sast-analysis-owasp-juice-shop/blob/main/report/report.md)
