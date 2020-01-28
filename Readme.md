# Multiple Transmitter Localization under Time-Skewed Observations

This project has been done to use the sensors reading(power values) in situation where sensors have a clock skew among each other so creating Observation Vector(O) may lead to error when we want to process their data for different purposes(e.g. Localization of Intruders(or TXs)).
The only information you should provide is the maximum possible skew between any pair of sensors.
The idea is creating a DAG with vertices(group) that represents a set of sensors such that they receive power from the same set of TXs and an edge e=(u, v) show group u hear from a set of TXs which is a subset of TXs that group v can receive signal from. Please refer to our IEEE DySPAN'19 conference paper below for more technical and theorical information. 

[Multiple Transmitter Localization under Time-Skewed Observations](https://www3.cs.stonybrook.edu/~mdasari/assets/pdf/dyspan19.pdf) [IEEE DySPAN 2019] 
 
To synchronize your data, first, you should provide power values for all the sensor in a window of time that is large enough. By being large, we mean that the more pulses the TXs in the field send, the more reliable output would be generated. A sensor's signal should be saved in a numpy array in which each element(sample) is the power value that its sensor receives in a short sensing window time scale. Note that sensing window is different from the window of observing. Sensing window is the smallest time unit here and it's the time that each power value is getting generated by sensors. Sensors create a power value(e.g. from FFT) for every sensing window. On the other hand, the large window our Synchronize needs is comprised of as many number of sensing window each represent one absolute sensing time for its sensor.

Then, and for each sensor, create a Sensor object and save the in a python list. Some of the parameters might be irrelevant for your porpuses but make sure you define noise_floor and/or accuracy_parameter. The first one(in dB) helps detecting non-noise pulses that are crucial for synchronizing because sequence of pulses is semi-id for a sensor. Because of possible noise existing in the sensors' observation you may want to increase(decrease) the noise floor by this factor.

Finally, create a Synchronize object, passing sensor list created in previous step and define three other parameters.
max_skew, as the only extra information about the sensors, is the maximum skew between any pair of sensors. This max_skew parameter should be in according to sensing window unit. For example, it should 50 if your sensors create power data every 10 microseconds and largest possible clock skew is in order of ~500 microseconds. minimum_interval as a parameter to define if two sensors have common set of TXs in their signal is the minimum number of pulses that a TX is sending. As mentioned before, the higher this number the more accurate and noise free results. Since Synchronize use the beginning and finishing events of pulses, provide a number less than(around 1.8) TWICE of minimum number of pulses for minimum_interval parameter. The last parameter, accuracy_similarity, is used when we want to define how similar two groups(or sensors) should be. 1 means totally similar and 0 means not similar at all.

The output is a list of SensorGroup object each related to a distinct set of TXs with its list of sensor and one observation vector that can be used instead of the whole observing window.
