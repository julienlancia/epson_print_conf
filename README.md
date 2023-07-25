# epson_print_conf
Epson Printer Configuration accessed via SNMP (TCP/IP)

## Installation

```
git clone https://github.com/Ircama/epson_print_conf
pip3 install easysnmp
cd epson_print_conf
```

## Usage

```
usage: epson_print_conf.py [-h] -m MODEL -a HOSTNAME [-i] [-q QUERY]
                           [--reset_waste_ink] [--brute-force-read-key] [-d]
                           [-e DUMP_EEPROM DUMP_EEPROM DUMP_EEPROM] [--dry-run]
                           [--write-first-ti-received-time FTRT FTRT FTRT]

optional arguments:
  -h, --help            show this help message and exit
  -m MODEL, --model MODEL
                        Printer model. Example: -m XP-205 (use ? to print all
                        supported models)
  -a HOSTNAME, --address HOSTNAME
                        Printer host name or IP address. (Example: -m 192.168.1.87)
  -i, --info            Print all available information and statistics (default
                        option)
  -q QUERY, --query QUERY
                        Print specific information. (Use ? to list all available
                        queries)
  --reset_waste_ink     Reset all waste ink levels to 0
  --brute-force-read-key
                        Detect the read_key via brute force
  -d, --debug           Print debug information
  -e DUMP_EEPROM DUMP_EEPROM DUMP_EEPROM, --eeprom-dump DUMP_EEPROM DUMP_EEPROM DUMP_EEPROM
                        Dump EEPROM (arguments: extension, start, stop)
  --dry-run             Dry-run change operations
  --write-first-ti-received-time FTRT FTRT FTRT
                        Change the first TI received time (arguments: year, month,
                        day)

Epson Printer Configuration accessed via SNMP (TCP/IP)
```

Examples:

```
# Print informations
python3 epson_print_conf.py -m XP-205 -a 192.168.1.87 -i

# Reset all waste ink levels to 0
python3 epson_print_conf.py -m XP-205 -a 192.168.1.87 --reset_waste_ink

# Change the first TI received time to 31 December 2016
python3 epson_print_conf.py -m XP-205 -a 192.168.1.87 --write-first-ti-received-time 2016 12 31

# Detect the read_key via brute force
python3 epson_print_conf.py -m XP-205 -a 192.168.1.87 --brute-force-read-key

# Only print status information
python3 epson_print_conf.py -m XP-205 -a 192.168.1.87 -q printer_status

# Only print SNMP 'MAC Address' name
python3 epson_print_conf.py -m XP-205 -a 192.168.1.87 -q 'MAC Address'

# Only print SNMP 'Lang 5' name
python3 epson_print_conf.py -m XP-205 -a 192.168.1.87 -q 'Lang 5'
```

## API Interface

```python
import epson_print_conf
printer = epson_print_conf.EpsonPrinter("XP-205", "192.168.1.87")

if not printer.parm:
    print("Unknown printer")
    quit()

stats = printer.stats()
print("stats:", stats)

ret = printer.session.get_snmp_info()
print("get_snmp_info:", ret)
ret = printer.session.get_serial_number()
print("get_serial_number:", ret)
ret = printer.session.get_firmware_version()
print("get_firmware_version:", ret)
ret = printer.session.get_printer_head_id()
print("get_printer_head_id:", ret)
ret = printer.session.get_cartridges()
print("get_cartridges:", ret)
ret = printer.session.get_printer_status()
print("get_printer_status:", ret)
ret = printer.session.get_ink_replacement_counters()
print("get_ink_replacement_counters:", ret)
ret = printer.session.get_waste_ink_levels()
print("get_waste_ink_levels:", ret)
ret = printer.session.get_last_printer_fatal_errors()
print("get_last_printer_fatal_errors:", ret)
ret = printer.session.get_stats()
print("get_stats:", ret)

printer.session.reset_waste_ink_levels()
printer.session.brute_force_read_key()
printer.session.write_first_ti_received_time(2000, 1, 2)
```

### Exceptions

```
TimeoutError
ValueError
```

## Output example

```
{'cartridges': ['18XL', '18XL', '18XL', '18XL'],
 'firmware_version': 'RF11I5 11 May 2018',
 'ink_replacement_counters': {('Black', '1B', 1),
                              ('Black', '1L', 19),
                              ('Black', '1S', 2),
                              ('Cyan', '1B', 1),
                              ('Cyan', '1L', 8),
                              ('Cyan', '1S', 1),
                              ('Magenta', '1B', 1),
                              ('Magenta', '1L', 6),
                              ('Magenta', '1S', 1),
                              ('Yellow', '1B', 1),
                              ('Yellow', '1L', 10),
                              ('Yellow', '1S', 1)},
 'last_printer_fatal_errors': ['08', 'F1', 'F1', 'F1', 'F1', '10'],
 'printer_head_id': '...',
 'printer_status': {'cancel_code': 'No request',
                    'ink_level': [(1, 89, 'Black'),
                                  (5, 77, 'Yellow'),
                                  (4, 59, 'Magenta'),
                                  (3, 40, 'Cyan')],
                    'jobname': 'Not defined',
                    'loading_path': 'fixed',
                    'maintenance_box_0': 'not full (0)',
                    'maintenance_box_1': 'not full (0)',
                    'paper_path': 'Cut sheet (Rear)',
                    'ready': True,
                    'replace_cartridge': '00000001',
                    'status': (4, 'Idle'),
                    'unknown': [('0x24', b'\x0f\x0f')]},
 'serial_number': '...',
 'snmp_info': {'Descr': 'EPSON Built-in 11b/g/n Print Server',
               'EEPS2 firmware version': 'EEPS2 Hard Ver.1.00 Firm Ver.0.50',
               'Emulation 1': 'unknown',
               'Emulation 2': 'ESC/P2',
               'Emulation 3': 'BDC',
               'Emulation 4': 'other',
               'Emulation 5': 'other',
               'Lang 1': 'unknown',
               'Lang 2': 'ESCPL2',
               'Lang 3': 'BDC',
               'Lang 4': 'D4',
               'Lang 5': 'ESCPR1',
               'MAC Address': '...',
               'Model': 'EPSON XP-205 207 Series',
               'Model short': 'XP-205 207 Series',
               'Name': '...',
               'Print input': 'Auto sheet feeder',
               'UpTime': '00:32:38'},
 'stats': {'First TI received time': '...',
           'Ink replacement cleaning counter': 78,
           'Manual cleaning counter': 129,
           'Timer cleaning counter': 4,
           'Total print page counter': 11504,
           'Total print pass counter': 510136,
           'Total scan counter': 4967},
 'waste_ink_levels': [90.45, 4.63]}
```

## Resources

### snmpget

Installation:

```
sudo apt-get install snmp
```

Usage:

```
# Read address 173.0
snmpget -v1 -d -c public 192.168.1.87 1.3.6.1.4.1.1248.1.2.2.44.1.1.2.1.124.124.7.0.25.7.65.190.160.173.0

# Read address 172.0
snmpget -v1 -d -c public 192.168.1.87 1.3.6.1.4.1.1248.1.2.2.44.1.1.2.1.124.124.7.0.25.7.65.190.160.172.0

# Write 25 to address 173.0
snmpget -v1 -d -c public 192.168.1.87 1.3.6.1.4.1.1248.1.2.2.44.1.1.2.1.124.124.16.0.25.7.66.189.33.173.0.25.88.98.108.98.117.112.99.106

# Write 153 to address 172.0
snmpget -v1 -d -c public 192.168.1.87 1.3.6.1.4.1.1248.1.2.2.44.1.1.2.1.124.124.16.0.25.7.66.189.33.172.0.153.88.98.108.98.117.112.99.106
```

### Other resources

epson-printer-snmp: https://github.com/Zedeldi/epson-printer-snmp

ReInkPy: https://codeberg.org/atufi/reinkpy/

ReInk: https://github.com/lion-simba/reink (especially https://github.com/lion-simba/reink/issues/1)

reink-net: https://github.com/gentu/reink-net

epson-l4160-ink-waste-resetter: https://github.com/nicootto/epson-l4160-ink-waste-resetter

emanage x900: https://github.com/abrasive/x900-otsakupuhastajat/

### Not used resources

Use at your risk.

WICreset: https://wic-reset.com / https://www.2manuals.com / https://resetters.com

The key, trial, can be used to reset your counters to 80%. After packet sniffing with wireshark, the correct OIDs can be found
This application also stores a log containing SNMP information at ~/.wicreset/application.log

Printhelp: https://printhelp.info/
