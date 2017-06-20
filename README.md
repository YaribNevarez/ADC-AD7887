# Analogue to digital converter (AD7887)

This class is the device driver for the ADC. As well as the AD5624R this class accomplish the SPI communication to the ADC device (AD7887). The class is defined in AD7887.h.
The SPI initialization is performed as exhibits the following function.

```C
inline static void AD7887_init_spi(void)
{
    while (!GET_ZYNQ_SPI_TRANSMISSION_DONE);
    SET_POXI_SPI_SLAVE_SELECT(ZYNQ_SPI_ADC_CS);
    SET_POXI_SPI_DATA_LENGTH(ZYNQ_SPI_DATA_LENGTH_16_BITS);
    SET_POXI_SPI_BAUD_RATE_DIVIDER(AD7887_SPI_BAUD_RATE);
    SET_POXI_SPI_CLOCK_POLARITY(1);
    SET_POXI_SPI_CLOCK_PHASE(0);
}
```

From this code is clearly seen the SPI configuration for the device: CPOL = 1, CPHA = 0, 16 bits.

Initialization code.
```C
static AD7887 * Poxi_ADC = AD7887_instance();
Poxi_ADC->set_reference(REF_ENABLED);
Poxi_ADC->set_channel_mode(SINGLE_CHANNEL);
Poxi_ADC->set_power_mode(MODE2);
```
The following table present the power mode options.

| Mode | Description |
|---|---|
|MODE1|The AD7887 enters shutdown if the CS input is 1 and is in full power mode when CS is 0.|
|MODE2|The AD7887 is always fully powered up, regardless of the status of any of the logic inputs.|
|MODE3|The AD7887 automatically enters shutdown mode at the end of each conversion, regardless of the state of CS.|
|MODE4|In this standby mode, portions of the AD7887 are powered down but the on-chip reference voltage remains powered up.|


The incoming code illustrates how to get the analogue voltage conversion.
```C
primitiveSignal = Poxi_ADC->read_analog();
```
 
The following code maintains in shutdowns condition in the ADC while its CS is in low state.
```C
Poxi_ADC->set_power_mode(MODE1);
```

For detailed information it can be referred the actual code.

Best regards,

-Yarib Nevárez
