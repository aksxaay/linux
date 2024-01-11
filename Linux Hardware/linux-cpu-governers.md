## Links
[konkor/cpufreq](https://konkor.github.io/cpufreq/faq/)
intel_pstate vs acpi-cpufreq

[CPUFreq governer // kernel documentation](https://www.kernel.org/doc/Documentation/cpu-freq/governors.txt)


[switching to acpi power](https://silvae86.github.io/2020/06/13/switching-to-acpi-power/#google_vignette)

[acpi-cpufreq vs intel_pstate](https://www.phoronix.com/review/intel_pstate_linux315)


make this a blog article pls

```sh
/etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="splash debug intel_pstate=disable"
```

right now running ACPI drivers, remove `intel_pstate=disable` to resume pstate drivers for intel