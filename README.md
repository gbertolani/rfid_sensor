# rfid_sensor

This is a Gazebo plugin for simulating RFID tags and RFID readers antennas. It has been tested on ROS 2 Humble and Gazebo Classic 11. Once the tag is within the sensing range of the reader antenna, the antenna acquires phase and RSSI signals along with the name ID of the tag. The reader antenna(s) can be configured specifying the frequency, the sensing range, and some noise values that are used to account for multipath effects and uncertainties in the measurement of phase and RSSI signals.

## Build

Place the package in a ROS 2 workspace's `src` directory. From the workspace root:

```bash
source /opt/ros/humble/setup.bash
colcon build --packages-select rfid_sensor
source install/setup.bash
```

## Usage
The antenna publishes `rfid_sensor/msg/TagArray` on `<sensor_name>/data` in the
namespace configured through the plugin's `<ros>` element. The message field
`antennaPose` is now `antenna_pose` to satisfy ROS 2 naming rules. Load Gazebo with
ROS 2 initialization (for example, through `gazebo_ros`). Set `use_sim_time` on
the plugin node when using simulation time.

The following example repository targets ROS 1 and requires migration to ROS 2.
If you want to use it check this example [repo](https://github.com/SalvatoreDAvella/rfid_simulator_test)

## Citing

If you use this tool in a research project, please check the [paper](https://doi.org/10.1109/ACCESS.2022.3152199) and cite it as follows:
```
@article{davella2022RFID,
  author={D’Avella, Salvatore and Unetti, Matteo and Tripicchio, Paolo},
  journal={IEEE Access}, 
  title={RFID Gazebo-Based Simulator With RSSI and Phase Signals for UHF Tags Localization and Tracking}, 
  year={2022},
  volume={10},
  number={},
  pages={22150-22160},
  publisher={IEEE},
  doi={10.1109/ACCESS.2022.3152199},
  issn={2169-3536}
}
```

## License
Distributed under the GNU License. See LICENSE for more information.
