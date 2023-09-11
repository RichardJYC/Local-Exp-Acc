CC = g++
CFLAGS = -std=c++14 -Wall -Wextra 
DEBUGFLAGS = -g -O0
RELEASEFLAGS = -O3
RM = rm -rf 
ECHO_OFF = @

LIBOPENCV_LIBS := /home/jyc/prj/opencv/lib/
LIBOPENCV_INC := /home/jyc/prj/opencv/include/
# vitis
LIBVITIS_INC := /home/jyc/prj/0GIT/stereo_project/lib/Vitis_HLS/
LIBVITIS_MATH := /home/jyc/prj/0GIT/stereo_project/prj_local_expansion_hls/cpp/c++/

CFLAGS += -fPIC -I$(LIBOPENCV_INC) -I$(LIBVITIS_INC) -L$(LIBOPENCV_LIBS) -lopencv_imgcodecs -lopencv_imgproc -lopencv_core -lopencv_highgui -lopencv_flann -lopencv_features2d

ifeq ($(KITTI), 1)
ADD_CFLAGS+=-DKITTI=1 -DLAYER1_GC=$(LAYER1_GC) -DLAYER2_GC=$(LAYER2_GC)
# ADD_CFLAGS+=-DCOST_CALC_MODE=0 -DCOST_LOAD_MODE=1 
ADD_CFLAGS+=-DWIDTH=1242 -DHEIGHT=375 -DDISP_MAX=192
ADD_CFLAGS+=-DLAYER0_ENABLE=1 -DLAYER0_SIZE=9
ADD_CFLAGS+=-DLAYER1_ENABLE=1 -DLAYER1_SIZE=18
ADD_CFLAGS+=-DLAYER2_ENABLE=0 -DLAYER2_SIZE=36
endif # KITTI

ifeq ($(MB), 1)
ADD_CFLAGS+=-DKITTI=0 -DLAYER1_GC=$(LAYER1_GC) -DLAYER2_GC=$(LAYER2_GC)
# ADD_CFLAGS+=-DCOST_CALC_MODE=0 -DCOST_LOAD_MODE=1 
ADD_CFLAGS+=-DWIDTH=$(WIDTH) -DHEIGHT=$(HEIGHT) -DDISP_MAX=$(DISP_MAX)
ADD_CFLAGS+=-DLAYER0_ENABLE=1 -DLAYER0_SIZE=9
ADD_CFLAGS+=-DLAYER1_ENABLE=1 -DLAYER1_SIZE=18
ADD_CFLAGS+=-DLAYER2_ENABLE=0 -DLAYER2_SIZE=36
endif # MB

CFLAGS += $(ADD_CFLAGS)

# source and destination file path
SRCDIR = ../cpp
BUILDDIR = ./build
DATADIR = ../../../KITTI/est_default/*/disp.png
SRC := $(filter-out $(SRCDIR)/local_exp_pipeline_hls_tb.cpp,$(wildcard $(SRCDIR)/*.cpp))
OBJ := $(patsubst $(SRCDIR)/%.cpp,$(BUILDDIR)/%.o,$(SRC))

CFLAGS += -I$(SRCDIR)/ 

default: release

# 编译规则
debug: CFLAGS += $(DEBUGFLAGS)
debug: all

release: CFLAGS += $(RELEASEFLAGS)
release: all

# 指定目标文件的输出位置
all: run

run: $(OBJ)
	$(CC) $(CFLAGS) $^ -o $@ `pkg-config opencv --cflags --libs` > info.log 2>&1

$(BUILDDIR)/%.o: $(SRCDIR)/%.cpp | $(BUILDDIR)
	$(CC) $(CFLAGS) -c $< -o $@ `pkg-config opencv --cflags --libs` > info.log 2>&1 

# 创建build文件夹
$(BUILDDIR):
	mkdir -p $@

clean:
	rm -rf $(BUILDDIR) run

.PHONY: clean

