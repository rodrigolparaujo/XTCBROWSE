# XTCBROWSE
Componente para gerar um TcBrowse Avançado com pesquisa, exportação para excel, legendas, checkbox, pontos de entrada,edição simples de campos, etc...

O nome do objeto é <b>__oLista</b>

O nome da variavel contendo o conteudo do array é <b>__aLista</b>

A variável <b>__nPosChkB</b> contém a posição do checkbox utilizado para selecionar o item, se não houver legendas, a posição é 1, caso contrário 2.

Para saber a quantidade de colunas no grid, use Len(__oLista:aColumns)

<p><center><img src="/Resources/xtcbrowser1.png"></center></p>

<p><center><img src="/Resources/xtcbrowser2.png"></center></p>

<p><center><img src="/Resources/xtcbrowser3.png"></center></p>

<p><center><img src="/Resources/botaodireito.png"></center></p>

<p><center><img src="/Resources/edicao de campos.gif"></center></p>

<p><center><img src="/Resources/selecao-avancada.gif"></center></p>

# Estrutura da função

```xbase

U_XBROWSE(oTela, _cAlias, _cQuery, _nIndice, _aIndice, _aFiltro, _aLegenda, _nAlinhamento, _nWidth, _nHeight, _nBotoes, _lVersao, _lLayout, _lCheckbox, _lSelecao  )

```

| 	Argumento	 | 	Finalidade	 | 	Obrigatório	 |  	Valor padrão	 | 
| 	:-----:	 | 	-----	 | 	:-----:	 | 	:-----:	 | 
| 	oTela	| 	Nome do objeto pai	| 	X	 | 	 	 | 
| 	_cAlias	| 	Alias da tabela	| 	X	 |  	 	 |
| 	_cQuery	| 	Query que será executada	| 	Opcional	 |  	 	 |
| 	_nIndice	| 	Número do indice da tabela, será usada para montar a pesquisa no grid	| 	Opcional	 |  	1	 |
| 	_aIndice	| 	Campos extras que irão complementar o indice	| 	Opcional	 |  	 	 |
| 	_aFiltro	| 	Campos extras que irão complementar o filtro que poderá ser usado na busca dos dados	| 	Opcional	 |  	 	 |
| 	_aLegenda	| 	Legenda	| 	Opcional	 |  	 	 |
| 	_nAlinhamento	| 	Alinhamento do componente na tela, podendo ser CONTROL_ALIGN_ALLCLIENT, CONTROL_ALIGN_TOP, CONTROL_ALIGN_BOTTOM, CONTROL_ALIGN_LEFT, CONTROL_ALIGN_RIGHT	| 	Opcional	 |  	CONTROL_ALIGN_ALLCLIENT	 |
| 	_nWidth	| 	Largura do componente	| 	Opcional	 |  	oTela:nWidth	 |
| 	_nHeight	| 	Altura do componente	| 	Opcional	 |  	oTela:nHeight	 |
| 	_nBotoes	| 	Posição dos botões na tela. Podendo ser 1 = Topo da janela, 2 = Rodape da janela	| 	Opcional	 |  	1	 |
| 	_lVersao	| 	Exibe um botão para mostrar a versão do componete. Se .T. mostra o menu para atualizar o componente	| 	Opcional	 |  	.T.	 |
| 	_lLayout	| 	Exibe uma tela com formatação usando CSS	| 	Opcional	 |  	.T.	 |
| 	_lCheckbox	| 	Exibe um checkbox na tela para seleção	| 	Opcional	 |  	.T.	 |
| 	_lSelecao	| 	Exibe uma tela de seleção de registros com mais recursos	| 	Opcional	 |  	.T.	 |

Variáveis públicas

| 	Variavel	 | 	Finalidade	 |
| 	:-----:	 | 	-----	 |
| 	__oLista	| 	Nome do controle	|
| 	__aLista	| 	Array contendo os registros do grid	|
| 	__cVersao	| 	Versão do componente	|
| 	__cUrlPatch	| 	Url para baixar a nova versão do componente	|


# Pontos de Entrada

## XBROWSEF
Ponto de entrada para complementar o array de Filtro

O array é composto por 2 partes:
- Campo
- Se .T. será montado um filtro de/até do campo. Se .F. terá exibido apenas o campo

```xbase

{"E2_EMISSAO",.T.}

```

Data Emissão de

Data Emissão até
  
```xbase
User Function XBROWSEF()
	Local aFiltro := aClone(PARAMIXB[1])
	
	aadd(aFiltro, {"E2_VALOR",.F.})
	aadd(aFiltro, {"E2_EMISSAO",.T.})
	aadd(aFiltro, {"E2_VENCTO",.T.})
	aadd(aFiltro, {"E2_VENCREA",.T.})

Return(aFiltro)
```

## XBROWSEI
Ponto de entrada usado para criar um índice que será usado para pesquisar no browse. Caso não seja informado, o indice será composto pelo número do índice ou por padrão será usado o indice 1 do alias informado.

```xbase
User Function XBROWSEI()
	Local aIndice := aClone(PARAMIXB[1])

	aadd(aIndice, "E2_VALOR")
	aadd(aIndice, "E2_EMISSAO")
	aadd(aIndice, "E2_VENCTO")
	aadd(aIndice, "E2_VENCREA")
	aadd(aIndice, "E2_HIST")
	aadd(aIndice, "E2_MOEDA")

Return(aIndice)
```

## XBROWSEL
Ponto de entrada para gerar a aLegenda 

O array é composto por 3 partes:
- Logica
- Titulo da legenda
- imagem da legenda

```xbase
User Function XBROWSEL()
	Local aLegenda := aClone(PARAMIXB[1])

	aadd(aLegenda, {"ROUND(SE2->E2_SALDO,2) == 0","Titulo Baixado","BR_VERMELHO"})
	aadd(aLegenda, {"ROUND(SE2->E2_SALDO,2) > 0 .AND. ROUND(SE2->E2_SALDO,2) <> ROUND(SE2->E2_VALOR,2)","Titulos Baixados Parcialmente","BR_AMARELO"})
	aadd(aLegenda, {"ROUND(SE2->E2_SALDO,2) == ROUND(SE2->E2_VALOR,2)","Titulos Pendentes","BR_VERDE"})

Return(aLegenda)
```

## XBROWSEB
Ponto de entrada para incluir botões no controle

O array é composto por 3 partes:
- Titulo do botão
- Função que será executa
- imagem do botão

<p><center><img src="/Resources/xtcbrowser3.png"></center></p>
<p><center><img src="/Resources/botaodireito.png"></center></p>

```xbase
User Function XBROWSEB()
	Local aBotoes := {}

	Aadd(aBotoes, {"Teste1", "MsgInfo('Teste1','Atenção')","MDISPOOL" })
	Aadd(aBotoes, {"Teste2", "MsgInfo('Teste2','Atenção')","LIMPEZA" })

Return(aBotoes)
```

## XBROWSEE
Ponto de entrada para permitir campos editáveis

O array é composto por 2 partes:
- Coluna que deseja a edição
- Se .T. é obrigatorio o valor

<p><center><img src="/Resources/edicao de campos2.png"></center></p>
  
```xbase
User Function XBROWSEE()
	Local aColunas := {}
	
	/*
	Argumentos:
	1 = Coluna que deseja editar
	2 = Se .T. é obrigatorio o valor
	*/

	Aadd(aColunas, {1,.T.})
	Aadd(aColunas, {2,.T.})
	Aadd(aColunas, {4,.T.})
	Aadd(aColunas, {6,.T.})
	Aadd(aColunas, {8,.T.})
	Aadd(aColunas, {10,.T.})
	Aadd(aColunas, {12,.T.})
	Aadd(aColunas, {13,.T.})
	Aadd(aColunas, {14,.T.})
	Aadd(aColunas, {15,.T.})
	
Return(aColunas)
```

## XBROWSEQ
Ponto de entrada para informar a query que será executada no controle

```xbase
User Function XBROWSEQ()
	Local cQuery := ""

	cQuery := " SELECT * FROM " + RetSqlName("SE2") + " "
	cQuery += " WHERE D_E_L_E_T_='' "
	cQuery += " AND E2_FILIAL = '" + xFilial("SE2") + "' "
	cQuery += " AND E2_EMISSAO >= '20240101' AND E2_PREFIXO = 'EIC'"
	cQuery += " ORDER BY E2_EMISSAO DESC, E2_NUM "

Return(cQuery)
```

# Na prática...

```xbase
User Function SeuPrograma()
	Local	_cAlias       := "SE2" //__cAlias (obrigatorio)
	Local	_cQuery       := "" //__cQuery (Opcional)
	Local	_nIndice      := 1 //__nIndice - Usado para ordenar os registros na tela (Opcional)
	Local	_aIndice      := {} //__aIndice - Campos para adicionar ao indice  (Opcional)
	Local	_aFiltro      := {} //{{"E2_VALOR",.F.},{"E2_EMISSAO",.T.},{"E2_VENCTO",.T.},{"E2_VENCREA",.F.}}	//__aFiltro - Campos para adicionar ao filtro  (Opcional) Posicão 1 = Campo, posição 2 = se irá ter de/ate
	Local	_aLegenda     := {} 
	Local	_nAlinhamento := CONTROL_ALIGN_ALLCLIENT //__nAlinhamento (Opcional)
	Local	_nWidth       := oTela2:nWidth //__nWidth (Opcional)
	Local	_nHeight      := oTela2:nHeight //__nHeight (Opcional)
	Local	_nBotoes      := 1 //1 - Botoes no topo, 2 - Botoes no rodape
	Local oPainel1      := Nil
	Local oEnviar       := Nil

	Static oTela

	DEFINE DIALOG oTela TITLE cCadastro FROM aSize[1],aSize[2] TO aSize[3],aSize[4] PIXEL
		oPainel1:= tPanel():New(0,0,"",oTela,,,,,CLR_WHITE,oScroll1:nWidth,45)
		oPainel1:Align := CONTROL_ALIGN_TOP
		
		oEnviar:= tButton():New(000,000, 'Gravar',oPainel1, {|| Gravar(), oTela:End()  },40,16,,,,.T.)
		
		U_XBROWSE(oTela /*oParent (obrigatorio)*/, _cAlias, _cQuery, _nIndice, _aIndice, _aFiltro, _aLegenda, _nAlinhamento, _nWidth, _nHeight, _nBotoes )
    
	ACTIVATE DIALOG oDlgTela CENTERED

Return

Static Function Gravar()
	Local i         := 0
	Local cTexto    := ""
	Local nPosCheck := __nPosChkB //Variavel public que indica a posição do Checkbox do browse
	Local nPosPre   := ""
	Local nPosNum   := ""
	Local nPosEmi   := ""
	Local nPosValor := 0
	Local nPosSaldo := 0

  //__aLista - array publico do componente contendo o resultado do browse.
  //__oLista - objeto publico do componente
  //Usando __oLista:aColumns[i]:CHEADING, você consegue identificar a posição do campo

	If Len(__aLista)==0
		MsgInfo("Nao existe dados para enviar!")
		Return
	Endif

	For i:= 1 To Len(__oLista:aColumns)
		Do Case
			Case Alltrim(__oLista:aColumns[i]:CHEADING) == Alltrim(GetSx3Cache("E2_PREFIXO", "X3_TITULO"))
				nPosPre := i
			Case Alltrim(__oLista:aColumns[i]:CHEADING) == Alltrim(GetSx3Cache("E2_NUM", "X3_TITULO"))
				nPosNum := i
			Case Alltrim(__oLista:aColumns[i]:CHEADING) == Alltrim(GetSx3Cache("E2_EMISSAO", "X3_TITULO"))
				nPosEmi := i
			Case Alltrim(__oLista:aColumns[i]:CHEADING) == Alltrim(GetSx3Cache("E2_VALOR", "X3_TITULO"))
				nPosValor := i
			Case Alltrim(__oLista:aColumns[i]:CHEADING) == Alltrim(GetSx3Cache("E2_SALDO", "X3_TITULO"))
				nPosSaldo := i
		End
	Next

	For i:= 1 To Len(__aLista)
		If __aLista[i,nPosCheck]
			cTexto += __aLista[i,nPosPre] + "-" +;
			__aLista[i,nPosNum] + "-" +;
			DTOC(__aLista[i,nPosEmi]) + "-" +;
			Transform(__aLista[i,nPosValor],PesqPict("SE2","E2_VALOR")) + "-" +;
			Transform(__aLista[i,nPosSaldo],PesqPict("SE2","E2_SALDO")) + CRLF
		Endif
	Next

	If !Empty(cTexto)
		MsgInfo(cTexto)
	Else
		MsgInfo("Nao existe dados para enviar!")
	Endif

Return
```
