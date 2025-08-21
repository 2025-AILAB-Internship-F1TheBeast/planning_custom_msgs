# Planning Custom Messages

Custom ROS2 message definitions for F1TENTH autonomous racing path planning and control.

## Message Types

### PathPoint
Single waypoint with velocity and curvature information.
```
float64 x              # x coordinate (m)
float64 y              # y coordinate (m) 
float64 yaw            # heading angle (rad)
float64 velocity       # target velocity (m/s)
float64 curvature      # path curvature (1/m)
float64 time_from_start # time from path start (s)
```

### PathWithVelocity
Complete trajectory with velocity profile for path following.
```
std_msgs/Header header  # timestamp and frame
PathPoint[] points      # trajectory waypoints
float64 max_velocity    # maximum allowed velocity (m/s)
uint32 path_id          # unique path identifier
```

## System Integration

```
Local Planner → PathWithVelocity → Path Follower → Vehicle Control
```

**Publishers:**
- `local_planner_pkg` → `/planned_path` (PathWithVelocity)

**Subscribers:** 
- `lattice_path_follower_f1tenth` ← `/planned_path` (PathWithVelocity)
- `data_collection_pkg_for_ml_local_planning` ← `/planned_path_with_velocity` (PathWithVelocity)

## Build

```bash
colcon build --packages-select planning_custom_msgs
source install/setup.bash
```

Build this package before any dependent packages.

## Dependencies

- `std_msgs`
- `builtin_interfaces`
- `nav_msgs`