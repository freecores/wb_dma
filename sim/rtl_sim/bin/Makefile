
SHELL = /bin/sh
#.QUIET

DMA_TARGETS=	../../../rtl/verilog/wb_dma_ch_arb.v		\
		../../../rtl/verilog/wb_dma_ch_pri_enc.v	\
		../../../rtl/verilog/wb_dma_ch_sel.v		\
		../../../rtl/verilog/wb_dma_top.v		\
		../../../rtl/verilog/wb_dma_ch_rf.v		\
		../../../rtl/verilog/wb_dma_rf.v		\
		../../../rtl/verilog/wb_dma_wb_if.v		\
		../../../rtl/verilog/wb_dma_wb_mast.v		\
		../../../rtl/verilog/wb_dma_wb_slv.v		\
		../../../rtl/verilog/wb_dma_de.v		\
		../../../rtl/verilog/wb_dma_inc30r.v		\


DMA_TB=		../../../bench/verilog/test_bench_top.v		\
		../../../bench/verilog/wb_slv_model.v		\
		../../../bench/verilog/wb_mast_model.v		\


TOP=test
INCDIR="-INCDIR ../../../rtl/verilog/ -INCDIR ../../../bench/verilog/"
#ACCESS="-ACCESS +r"
#WAVES=-DEFINE WAVES

dmawxl:
	verilog +incdir+verilog/ $(DMA_TARGETS) $(DMA_TB)

dmaw:
	@$(MAKE) -s dma ACCESS="-ACCESS +r" WAVES="-DEFINE WAVES"

dmass:
	signalscan -do waves/dma.do -waves waves/waves.trn &

dma:
	@$(MAKE) -s vlog TARGETS="$(DMA_TARGETS) $(DMA_TB)" INCDIR=$(INCDIR) WAVES="$(WAVES)" TOP=test
	@$(MAKE) -s elab ACCESS="$(ACCESS)" TOP=test
	@$(MAKE) -s sim  TOP=test

clean:
	rm -f .nclog ./waves/*.dsn ./waves/*.trn ./ncwork/inca.linux.* ./ncwork/.inc*

simx:
	ncverilog +incdir+verilog +incdir+test_bench $(DMA_TARGETS) $(DMA_TB)

hal:
	hal +incdir+./verilog/ -NOP -NOS -nocheck STYVAL:USEPRT:NOBLKN:DLNBLK $(DMA_TARGETS)

#################################################################################
#
# NCVLOG
#
#################################################################################

vlog:
	@ncvlog -CDSLIB ncwork/cds.lib -HDLVAR ncwork/hdl.var	\
		-WORK ncwork -NOCOPYRIGHT -UPDATE	\
		-LOGFILE .nclog $(WAVES) $(TARGETS) $(INCDIR)

#################################################################################
#
# NCELAB
#
#################################################################################

elab:
	@ncelab -CDSLIB ncwork/cds.lib -HDLVAR ncwork/hdl.var	 \
		-WORK ncwork -NOCOPYRIGHT $(ACCESS) \
		-LOGFILE .nclog -APPEND_LOG -NOTIMINGCHECKS	 \
		ncwork.$(TOP)

#################################################################################
#
# NCSIM
#
#################################################################################

sim:
	@ncsim -CDSLIB ncwork/cds.lib -HDLVAR ncwork/hdl.var 	\
		-NOCOPYRIGHT -STATUS -LOGFILE .nclog -EXIT	\
		-APPEND_LOG -ERRORMAX 10 -NOKEY -UPDATE	\
		ncwork.$(TOP)

