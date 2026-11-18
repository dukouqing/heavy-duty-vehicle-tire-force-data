# 图4—图7及MAT源数据

Figures 4–7 and MATLAB Source Data

## 中文说明

### 1. 数据与图片
本数据包提供论文图4—图7的PDF矢量图、PNG图片及MAT源数据，涵盖双移线和鱼钩两项工况。每张图对应一个MAT文件。

图4：双移线，路面附着系数0.5，当前轮胎力估计。
`MAT_Data/Figure_04_DLC_mu0p5_Estimation_data.mat`

图5：双移线，路面附着系数0.5，0.2秒预测及稳定性分析。
`MAT_Data/Figure_05_DLC_mu0p5_Prediction_data.mat`

图6：鱼钩，路面附着系数0.8，当前轮胎力估计。
`MAT_Data/Figure_06_Fishhook_mu0p8_Estimation_data.mat`

图7：鱼钩，路面附着系数0.8，0.2秒预测及稳定性分析。
`MAT_Data/Figure_07_Fishhook_mu0p8_Prediction_data.mat`

对应图片存放在Figures文件夹，文件名与上述MAT文件一致，去掉_data后分别使用.pdf和.png后缀。

### 2. 方法与数组顺序
当前力估计的方法顺序：Proposed（本文方法）、LSTM、GRU、CNN、PINN-MLP、DICO。

预测的方法顺序：Proposed、LSTM、GRU、CNN、PINN-MLP。

model_names记录方法顺序。GRU预测基线采用GRU编码器和LSTM解码器。

三维输出数组按“时间 × 信号 × 方法”排列；二维参考数组按“时间 × 信号”排列。

wheel_order为L1、R1、L2、R2、L3、R3，分别表示前轴左/右轮、中轴左/右轮、后轴左/右轮。

18维力的列顺序为六轮Fz、六轮Fx、六轮Fy，单位均为N。

图中力曲线展示左前轮L1，其Fz、Fx、Fy分别对应第1、7、13列。六轮垂向力用于计算LTR。

以上列号采用MATLAB从1开始的索引。

### 3. 当前力估计变量（图4、图6）
time_s：当前估计时间，单位s。

reference_forces_N：六轮轮胎力参考值，维度为N×18。

estimated_forces_N：六种方法的六轮轮胎力估计值，维度为N×18×6。

error_forces_N：轮胎力估计误差，维度为N×18×6。

reference_LTR：参考横向载荷转移率，长度为N。

estimated_LTR：六种方法的横向载荷转移率，维度为N×6。

error_LTR：横向载荷转移率误差，维度为N×6。

这里N表示时间样本数。图4为3902个样本，图6为3402个样本。

### 4. 预测与稳定性变量（图5、图7）
target_time_s：预测目标时间，单位s。

forecast_origin_time_s：预测起始时间，等于目标时间减0.2s。

true_prediction_20D：20维状态参考值，维度为N×20。

prediction_20D：五种方法的20维状态预测值，维度为N×20×5。

error_prediction_20D：20维状态预测误差，维度为N×20×5。

第1—18列为六轮力，第19列为质心侧偏角Beta（°），第20列为横摆角速度Yaw（°/s）。

prediction_column_names和prediction_column_units分别给出逐列变量名和单位。

预测数据记录每个起始时刻向前递推20步、即0.2s末端的结果，并按预测目标时间对齐。

reference_LTR、predicted_LTR和error_LTR分别为参考LTR、预测LTR及其误差。

proposed_Beta_deg、proposed_Yaw_deg_s和proposed_LTR给出图5、图7中(g)、(h)的本文方法轨迹。

proposed_L1_Fx_over_muFz和proposed_L1_Fy_over_muFz给出(i)的左前轮力利用率坐标，分别为Fx/(μFz)和Fy/(μFz)。

road_friction_mu记录路面附着系数。稳定域边界及分区见对应PDF图片。

这里N表示时间样本数。图5为3882个样本，图7为3382个样本。

### 5. 计算约定
误差统一定义为“方法输出减参考值”，单位与对应信号相同。

LTR = (左侧三轮Fz之和 − 右侧三轮Fz之和) / 六轮Fz总和，为无量纲量。

LTR由六轮原始垂向力数值直接计算。时间保留源数据精度。

### 6. 读取示例
在解压后的数据包根目录运行MATLAB：
```matlab
S = load('MAT_Data/Figure_04_DLC_mu0p5_Estimation_data.mat');
t = S.time_s;
Fx_L1_Proposed = S.estimated_forces_N(:,7,1);
Fx_L1_Reference = S.reference_forces_N(:,7);
plot(t,Fx_L1_Reference,t,Fx_L1_Proposed);
xlabel('Time (s)'); ylabel('Fx L1 (N)');
legend('Reference','Proposed');
```

MAT文件也可通过Python的scipy.io.loadmat读取。SHA256SUMS.json提供文件校验值，validation.json记录数据核验结果。

## English Description

### 1. Data and figures
This dataset provides vector PDF figures, PNG images and MATLAB source data for Figures 4–7 of the manuscript, covering the double-lane-change and fishhook maneuvers. Each figure has a corresponding MAT file.

Figure 4: double-lane change, road friction coefficient 0.5, current tire-force estimation.
`MAT_Data/Figure_04_DLC_mu0p5_Estimation_data.mat`

Figure 5: double-lane change, road friction coefficient 0.5, 0.2 s prediction and stability analysis.
`MAT_Data/Figure_05_DLC_mu0p5_Prediction_data.mat`

Figure 6: fishhook, road friction coefficient 0.8, current tire-force estimation.
`MAT_Data/Figure_06_Fishhook_mu0p8_Estimation_data.mat`

Figure 7: fishhook, road friction coefficient 0.8, 0.2 s prediction and stability analysis.
`MAT_Data/Figure_07_Fishhook_mu0p8_Prediction_data.mat`

The Figures directory contains the corresponding images. Their filenames match the MAT filenames after removing _data and using the .pdf or .png extension.

### 2. Methods and array order
Current-force estimation methods are ordered as Proposed, LSTM, GRU, CNN, PINN-MLP and DICO.

Prediction methods are ordered as Proposed, LSTM, GRU, CNN and PINN-MLP.

model_names stores this order. The GRU prediction baseline uses a GRU encoder and an LSTM decoder.

Three-dimensional output arrays follow time × signal × method; two-dimensional reference arrays follow time × signal.

wheel_order is L1, R1, L2, R2, L3, R3: left/right wheels on the front, middle and rear axles, respectively.

The 18 force columns contain Fz for all six wheels, followed by Fx for all six wheels and Fy for all six wheels, in N.

Force curves show the left-front wheel L1. Its Fz, Fx and Fy occupy columns 1, 7 and 13. All six vertical wheel forces are used to calculate LTR.

Column numbers use MATLAB one-based indexing.

### 3. Current-force estimation variables (Figures 4 and 6)
time_s: current estimation time, in s.

reference_forces_N: six-wheel reference forces, shape N×18.

estimated_forces_N: six-wheel force estimates from the six methods, shape N×18×6.

error_forces_N: force estimation errors, shape N×18×6.

reference_LTR: reference lateral load-transfer ratio, length N.

estimated_LTR: lateral load-transfer ratios for the six methods, shape N×6.

error_LTR: lateral load-transfer ratio errors, shape N×6.

N denotes the number of time samples: 3902 for Figure 4 and 3402 for Figure 6.

### 4. Prediction and stability variables (Figures 5 and 7)
target_time_s: forecast target time, in s.

forecast_origin_time_s: forecast origin time, equal to target time minus 0.2 s.

true_prediction_20D: reference 20-dimensional state, shape N×20.

prediction_20D: predicted 20-dimensional states from the five methods, shape N×20×5.

error_prediction_20D: state prediction errors, shape N×20×5.

Columns 1–18 contain the six-wheel forces. Column 19 is the vehicle sideslip angle Beta in degrees; column 20 is the yaw rate Yaw in degrees/s.

prediction_column_names and prediction_column_units provide the individual variable names and units.

Each forecast records the terminal result after 20 recursive steps, corresponding to 0.2 s, and is aligned to its target time.

reference_LTR, predicted_LTR and error_LTR contain the reference LTR, predicted LTR and LTR errors.

proposed_Beta_deg, proposed_Yaw_deg_s and proposed_LTR provide the Proposed trajectories in panels (g) and (h) of Figures 5 and 7.

proposed_L1_Fx_over_muFz and proposed_L1_Fy_over_muFz provide the left-front tire-force utilization coordinates in panel (i), Fx/(μFz) and Fy/(μFz), respectively.

road_friction_mu stores the road friction coefficient. Stability boundaries and regions appear in the corresponding PDF figures.

N denotes the number of time samples: 3882 for Figure 5 and 3382 for Figure 7.

### 5. Calculation conventions
Errors are defined as method output minus reference and use the units of the corresponding signals.

LTR = (sum of left-wheel Fz − sum of right-wheel Fz) / sum of all six Fz; LTR is dimensionless.

LTR is calculated directly from the original six-wheel vertical force values. Timestamps retain source-data precision.

### 6. Reading example
Run the following MATLAB commands from the root of the extracted dataset:
```matlab
S = load('MAT_Data/Figure_04_DLC_mu0p5_Estimation_data.mat');
t = S.time_s;
Fx_L1_Proposed = S.estimated_forces_N(:,7,1);
Fx_L1_Reference = S.reference_forces_N(:,7);
plot(t,Fx_L1_Reference,t,Fx_L1_Proposed);
xlabel('Time (s)'); ylabel('Fx L1 (N)');
legend('Reference','Proposed');
```

The MAT files can also be read with scipy.io.loadmat in Python. SHA256SUMS.json provides file checksums, and validation.json records the data verification results.
