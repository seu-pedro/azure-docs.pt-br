---
title: Arquitetura de conectividade-banco de dados do Azure para PostgreSQL-servidor único
description: Descreve a arquitetura de conectividade do banco de dados do Azure para PostgreSQL-servidor único.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 05/23/2019
ms.openlocfilehash: 956c734214f3ecff532d2a73f069ecaa37396109
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/26/2020
ms.locfileid: "76760531"
---
# <a name="connectivity-architecture-in-azure-database-for-postgresql"></a>Arquitetura de conectividade no banco de dados do Azure para PostgreSQL
Este artigo explica a arquitetura de conectividade do banco de dados do Azure para PostgreSQL e também como o tráfego é direcionado para a instância do banco de dados do Azure para PostgreSQL de clientes dentro e fora do Azure.

## <a name="connectivity-architecture"></a>Arquitetura de conectividade
A conexão com o banco de dados do Azure para PostgreSQL é estabelecida por meio de um gateway que é responsável por rotear conexões de entrada para o local físico do servidor em nossos clusters. O diagrama a seguir ilustra o fluxo de tráfego.

![Visão geral da arquitetura de conectividade](./media/concepts-connectivity-architecture/connectivity-architecture-overview-proxy.png)

Como o cliente se conecta ao banco de dados, ele obtém uma cadeia de conexão que se conecta ao gateway. Esse gateway tem um endereço IP público que escuta a porta 5432. Dentro do cluster de banco de dados, o tráfego é encaminhado para o banco de dados do Azure apropriado para PostgreSQL. Portanto, para se conectar ao servidor, como de redes corporativas, é necessário abrir o Firewall do lado do cliente para permitir que o tráfego de saída seja capaz de alcançar nossos gateways. Abaixo, você pode encontrar uma lista completa dos endereços IP usados por nossos gateways por região.

## <a name="azure-database-for-postgresql-gateway-ip-addresses"></a>Banco de dados do Azure para os endereços IP do gateway PostgreSQL
A tabela a seguir lista os IPs primários e secundários do gateway do banco de dados do Azure para PostgreSQL para todas as regiões de data. O endereço IP primário é o endereço IP atual do gateway e o segundo endereço IP é um endereço IP de failover em caso de falha do primário. Conforme mencionado, os clientes devem permitir a saída para os dois endereços IP. O segundo endereço IP não escuta em nenhum serviço até que seja ativado pelo banco de dados do Azure para PostgreSQL para aceitar conexões.

| **Nome da região** | **Endereços IP do gateway** |
|:----------------|:-------------|
| Austrália Central| 20.36.105.0     |
| Central2 da Austrália     | 20.36.113.0   |
| Austrália Oriental | 13.75.149.87, 40.79.161.1     |
| Sudeste da Austrália |191.239.192.109, 13.73.109.251   |
| Sul do Brasil | 104.41.11.5, 191.233.201.8, 191.233.200.16  |
| Canadá Central |40.85.224.249  |
| Leste do Canadá | 40.86.226.166    |
| EUA Central | 23.99.160.139, 13.67.215.62   |
| Leste da China | 139.219.130.35    |
| Leste da China 2 | 40.73.82.1  |
| Norte da China | 139.219.15.17    |
| Norte da China 2 | 40.73.50.0     |
| Ásia Oriental | 191.234.2.139, 52.175.33.150, 13.75.33.20, 13.75.33.21     |
| Leste dos EUA | 40.121.158.30, 191.238.6.43  |
| Leste dos EUA 2 |40.79.84.180, 191.239.224.107, 52.177.185.181   |
| França Central | 40.79.137.0, 40.79.129.1  |
| Alemanha Central | 51.4.144.100     |
| Nordeste da Alemanha | 51.5.144.179  |
| Centro da Índia | 104.211.96.159     |
| Sul da Índia | 104.211.224.146  |
| Oeste da Índia | 104.211.160.80    |
| Leste do Japão | 13.78.61.196, 191.237.240.43  |
| Oeste do Japão | 104.214.148.156, 191.238.68.11    |
| Coreia Central | 52.231.32.42   |
| Sul da Coreia | 52.231.200.86    |
| Centro-Norte dos EUA | 23.96.178.199, 23.98.55.75, 52.162.104.35, 52.162.104.36    |
| Europa Setentrional | 40.113.93.91, 191.235.193.75    |
| Norte da África do Sul  | 102.133.152.0    |
| Oeste da África do Sul | 102.133.24.0   |
| Centro-Sul dos EUA |13.66.62.124, 23.98.162.75   |
| Sudeste Asiático | 104.43.15.0, 23.100.117.95, 40.78.233.2, 23.98.80.12     |
| EAU Central | 20.37.72.64  |
| Norte dos EAU | 65.52.248.0    |
| Sul do Reino Unido | 51.140.184.11   |
| Oeste do Reino Unido | 51.141.8.11  |
| Centro-Oeste dos EUA | 13.78.145.25     |
| Oeste da Europa | 40.68.37.158, 191.237.232.75     |
| Oeste dos EUA | 104.42.238.205, 23.99.34.75  |
| Oeste dos EUA 2 | 13.66.226.202  |
||||

## <a name="next-steps"></a>Próximos passos

* [Criar e gerenciar regras de firewall do Banco de Dados do Azure para PostgreSQL usando o Portal do Azure](./howto-manage-firewall-using-portal.md)
* [Criar e gerenciar regras de firewall do Banco de Dados do Azure para PostgreSQL usando a CLI do Azure](./howto-manage-firewall-using-cli.md)
