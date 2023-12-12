# Casos de Dengue no Rio de Janeiro
Projeto inicialmente desenvolvido como preparação para um processo seletivo. Neste estudo fica evidenciado o aumento do número de casos notificados no estado do RJ e propõe uma associação entre número de casos versus situação climática (temperatura, precipitação e umidade relativa do ar).

## Problemática
A base de dados compreende o período de janeiro/2014 até julho/2023, retirados do DataSUS em novembro/2023.  
Apenas no ano de 2023, durante 7 meses, foram notificados mais de 33 mil casos, alcançando a marca de 180% maior que o somatório dos 3 anos anteriores (total de 18,3 mil notificações de jan/2020 à dez/2022).  
Além de campanhas estaduais que combatem diretamente este problema de saúde pública, trago uma proposta de criar um modelo preditivo que seja capaz de acertar a quantidade de casos, associando este número a situação climática.

### Documentação
Todas as considerações feitas e dificuldades enfrentadas para desenvolver este projeto você encontra exatamente [aqui](https://github.com/jeantorre/casos_dengue_RJ/blob/main/documents/README_dengue_RJ.md).  

## Requirements.txt
autots              0.6.3  
matplotlib          3.8.2  
numpy               1.26.2  
pandas              2.1.3  
prophet             1.1.5  
seaborn             0.13.0  
session_info        1.0.0

## Estudos futuros
A quantidade de informações climáticas, por si só, não trouxeram resultados muito satisfatórios. Como sugestão futura para o desenvolvimento deste algortimo temos:  
- Uma coluna com uma pontuação quando o estado estiver em realizando campanha para o combate a dengue dentro de determinado período. Por exemplo, 0 se não estiver em camapnhas e 1 quando estiver em campanhas.
- Realizar um estudo sobre o mosquito aedes aegypti e verificar qual a temperatura e umidade ótima para desenvolvimento da larva até sua fase adulta. Desta forma também criar um sistema de pontuações caso o período esteja respeitando essa faixa. Em contra partida, caso a temperatura não auxilie o desenvolvimento das larvas, ter pontuações nulas ou negativas.

O intuito dessas sugestões é criar um sistema de recompensa para o modelo de machine learning, tentando trazer resultados mais próximos da realidade.

---

# Referência
- Dengue - Notificações registradas no sistema de informação de agravos de notificação - Rio de Janeiro. **DataSUS**, 2023. Disponível em: <http://tabnet.datasus.gov.br/cgi/tabcgi.exe?sinannet/cnv/denguebrj.def>. Acesso em 08 de dez. de 2023.
- Instituto Nacional de Meteorologia. **INMET**, 2023. Disponível em: <https://bdmep.inmet.gov.br/>. Acesso em 08 de dez. de 2023.
- Secretaria de Saúde promove ações de prevenção à dengue em pontos estratégicos. **Rio Prefeitura**, 2023. Disponível em: <https://prefeitura.rio/saude/secretaria-de-saude-promove-acoes-de-prevencao-a-dengue-em-pontos-estrategicos/>. Acesso em 11 de dez. de 2023.

---

[![Linkedin](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/jean-torre-44a27914b/)
[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://www.instagram.com/torrej/)
[![Gmail](https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:jean.torre21@gmail.com)
