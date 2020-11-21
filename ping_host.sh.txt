#!/bin/bash
############################################################################
# Monitoraramento de host baseado em resposta de ping
# pingHOST.sh - v.0.1 - 2011/03/01
#
# Autor: Alexsandro Felix
# Site: http://blog.ffelix.eti.br
# E-mail/MSN/GTalk: felix@ffelix.eti.br
# Este script pode ser copiado e modificado livremente,
# desde que os devidos créditos sejam concedam ao autor os devidos créditos
# O script original pode ser encontrado em: http://blog.ffelix.eti.br/monitorar-hosts-por-meio-de-ping/
############################################################################
# Quantia de ping a serem enviados para cada host
COUNT=4
for hosts in $(cat hosts.txt); do
	# email report when
	SUBJECT="#Falha de ping"
	EMAILID="email@dominio.com"
	for myHost in $hosts do
		count=$(ping -c $COUNT $myHost | grep 'received' | awk -F',' '{ print $2 }'| awk '{ print $1 }')

		if [ $count -eq 0 ]; then
			echo "RB : $myHost apresenta-se offline em $(date)" | mail -s "$SUBJECT" $EMAILID
			echo "Rb: $myHost apresenta-se offline em $(date)" >> ping.log
		fi
	done
done