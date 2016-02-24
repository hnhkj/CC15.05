/dts-v1/;

/include/ "mt7628an.dtsi"

/ {
	compatible = "mediatek,linkit", "mediatek,mt7628an-soc";
	model = "MediaTek LinkIt Smart 7688";

	chosen {
		bootargs = "console=ttyS2,57600";
	};

	memory@0 {
		device_type = "memory";
		reg = <0x0 0x8000000>;
	};

	pinctrl {
		state_default: pinctrl0 {
			gpio {
				ralink,group = "gpio";
				ralink,function = "gpio";
			};
			perst {
				ralink,group = "perst";
				ralink,function = "gpio";
			};
			refclk {
				ralink,group = "refclk";
				ralink,function = "gpio";
			};
			i2s {
				ralink,group = "i2s";
				ralink,function = "gpio";
			};
			spis {
				ralink,group = "spis";
				ralink,function = "gpio";
			};
			wled_kn {
				ralink,group = "wled_kn";
				ralink,function = "gpio";
			};
			wled_an {
				ralink,group = "wled_an";
				ralink,function = "wled_an";
			};
			wdt {
				ralink,group = "wdt";
				ralink,function = "gpio";
			};
		};
	};

	palmbus@10000000 {
		spi@b00 {
			status = "okay";

			pinctrl-names = "default";
			pinctrl-0 = <&spi_pins>, <&spi_cs1_pins>;

			m25p80@0 {
				#address-cells = <1>;
				#size-cells = <1>;
				compatible = "mx25l25635e";
				reg = <0 0>;
				linux,modalias = "m25p80", "mx25l25635e";
				spi-max-frequency = <40000000>;
				m25p,chunked-io = <31>;

				partition@0 {
					label = "u-boot";
					reg = <0x0 0x30000>;
					read-only;
				};

				partition@30000 {
					label = "u-boot-env";
					reg = <0x30000 0x10000>;
				};

				factory: partition@40000 {
					label = "factory";
					reg = <0x40000 0x10000>;
					//read-only;
				};

				partition@50000 {
					label = "firmware";
					reg = <0x50000 0x1fb0000>;
				};
			};

			spidev@1 {
				#address-cells = <1>;
				#size-cells = <1>;
				compatible = "spidev";
				reg = <1 0>;
				spi-max-frequency = <40000000>;
			};
		};

		i2c@900 {
			status = "okay";
		};

		uart1@d00 {
			status = "okay";
		};

		uart2@e00 {
			status = "okay";
		};

		pwm@5000 {
			status = "disabled";
		};
	};

	ethernet@10100000 {
		mtd-mac-address = <&factory 0x28>;
	};

	sdhci@10130000 {
		status = "okay";
		mediatek,cd-low;
//		mediatek,cd-poll;
	};

	bootstrap {
		compatible = "mediatek,linkit";

		status = "okay";
	};

	gpio-leds {
		compatible = "gpio-leds";
		wifi {
			label = "mediatek:orange:wifi";
			gpios = <&wgpio 0 0>;
			default-state = "on";
		};
	};

	gpio-keys-polled {
		compatible = "gpio-keys-polled";
		#address-cells = <1>;
		#size-cells = <0>;
		poll-interval = <20>;
		wps {
			label = "reset";
			gpios = <&gpio1 6 1>;
			linux,code = <0x211>;
		};
		BTN_0 {
			label = "BTN_0";
			gpios = <&gpio0 14 1>;		//GPIO14-S4
			linux,code = <0x100>;
		};
		BTN_1 {
			label = "BTN_1";
			gpios = <&gpio0 15 1>;		//GPIO15-S5
			linux,code = <0x101>;
		};
		BTN_2 {
			label = "BTN_2";
			gpios = <&gpio0 16 1>;		//GPIO16-S6
			linux,code = <0x102>;
		};
		BTN_3 {
			label = "BNT_3";
			gpios = <&gpio0 17 1>;		//GPIO17-S7
			linux,code = <0x103>;
		};
		BTN_4 {
			label = "BTN_4";
			gpios = <&gpio0 18 1>;		//GPIO18-S8
			linux,code = <0x104>;
		};
		BTN_5 {
			label = "S9";
			gpios = <&gpio0 19 1>;		//GPIO19-S9
			linux,code = <0x105>;
		};
	};

	wgpio: gpio-wifi {
		#address-cells = <1>;
		#size-cells = <0>;

		compatible = "mediatek,gpio-wifi";
		gpio-controller;
		#gpio-cells = <2>;
	};

};
