# ABAP-ALV-HotSpot
Call a transaction or program from ALV Reporte
CALL FUNCTION 'REUSE_ALV_GRID_DISPLAY'
  EXPORTING
    i_callback_program = v_repid
    i_callback_user_command = 'USER_COMMAND'
    it_fieldcat        = it_catalog
    is_layout          = st_layout
  TABLES
    t_outtab           = it_pro.


FORM user_command  USING ucomm    LIKE sy-ucomm
                         selfield TYPE slis_selfield.
* Submit ir a la SE93 para ver el programa que llama la tx
* CALL ir a la SE11 y buscar el ID del parametro
  IF selfield-tabindex NE 0.
    READ TABLE it_pro INDEX selfield-tabindex INTO wa_end2.
    IF sy-subrc EQ 0.
      IF selfield-fieldname EQ 'LBKUM'.
        SET PARAMETER ID 'MAT' FIELD wa_end2-MATNR.
        SET PARAMETER ID 'WRK' FIELD wa_end2-BWKEY.
        CALL TRANSACTION 'MB52'.

      ENDIF.
      IF selfield-fieldname EQ 'DIAS'.
        SET PARAMETER ID 'MAT' FIELD wa_end2-MATNR.
        SET PARAMETER ID 'WRK' FIELD wa_end2-BWKEY.
        SET PARAMETER ID 'BWA' FIELD '251'.
        CALL TRANSACTION 'MB51'.

      ENDIF.

    ENDIF.
  ENDIF.
ENDFORM.
