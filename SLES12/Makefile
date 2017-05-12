PROJECT_DIR = $(CURDIR)
BUILD_DIR = /tmp/SLES12_root
OUTPUT_DIR = /tmp/SLES12_output

build :: clean
	sudo /usr/sbin/kiwi --prepare $(PROJECT_DIR) --root $(BUILD_DIR)
	mkdir -p $(OUTPUT_DIR)
	sudo /usr/sbin/kiwi --create $(BUILD_DIR) --type docker -d $(OUTPUT_DIR)

clean ::
	sudo rm -rf $(BUILD_DIR)
	sudo rm -rf $(OUTPUT_DIR)
	sudo rm -f $(BUILD_DIR).log
