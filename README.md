# RelatorioConsultaKIT
Relatorio para colsulta de Kits Sistemas MV Soul
select  itmvto.cd_produto
    ,mvto.cd_estoque
    ,mvto.cd_mvto_estoque
    ,itmvto.cd_lote
    ,itmvTo.dt_validade
    ,identificador_etiqueta.sn_ativo
    ,mvto.sn_kit_armazenado
    ,Sum(itmvto.qt_movimentacao * u.vl_fator) OVER() qt_teste
    ,itmvto.qt_movimentacao * u.vl_fator qt_produzida
    ,lot.qt_kit qt_reservada
    ,lot.qt_estoque_atual
    ,identificador_etiqueta.cd_identificador
    ,mvto.dsp_cd_barras

from dbamv.itmvto_kit_produzido itmvto,
    dbamv.mvto_kit_produzido mvto,
    dbamv.produto prod,
    dbamv.identificador_etiqueta,
    dbamv.lot_pro lot,
    dbamv.uni_pro u

where mvto.cd_mvto_estoque = itmvto.cd_mvto_estoque
      and prod.cd_produto = mvto.cd_kit
      AND u.cd_uni_pro = itmvto.cd_uni_pro
      and identificador_etiqueta.cd_identificador = dbamv.fnc_mges_valida_cod_barra(mvto.dsp_cd_barras)
      AND lot.cd_produto = itmvto.cd_produto
      AND lot.cd_estoque = mvto.cd_estoque
      AND Nvl(lot.cd_lote,'XXXXXX') = Nvl(itmvto.cd_lote,'XXXXXX')
      AND Nvl(lot.dt_validade,To_Date('01/01/3000','DD/MM/YYYY')) = Nvl(itmvTo.dt_validade,To_Date('01/01/3000','DD/MM/YYYY')  )

   -- A PARTIR DAQUI, MODIFICAR CONFORME O QUE DESEJA VERIFICAR
    AND itmvto.cd_produto = 
    --AND mvto.cd_kit = 16256 -- AQUI
    AND lot.cd_lote = 
    --AND lot.dt_validade = To_Date('30/04/2021','DD/MM/YYYY')
    --AND mvto.cd_mvto_estoque in (7098159,7131963)
    AND lot.cd_estoque = 
    --AND identificador_etiqueta.cd_identificador = dbamv.fnc_mges_valida_cod_barra('2000000215358')
    AND mvto.sn_kit_armazenado = 'S' AND identificador_etiqueta.sn_ativo = 'S'
    --and (mvto.sn_kit_armazenado  = 'S' or identificador_etiqueta.sn_ativo = 'S')

