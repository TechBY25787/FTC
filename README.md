# FIRST Tech Challenge (FTC)

Establish FTC database for Bayi School
# FTC TechBY 25787 China
# 23-24 Center Stage 中央舞台开源
// /* Copyright (c) 2017 FIRST. All rights reserved.
//  *
//  * Redistribution and use in source and binary forms, with or without modification,
//  * are permitted (subject to the limitations in the disclaimer below) provided that
//  * the following conditions are met:
//  *
//  * Redistributions of source code must retain the above copyright notice, this list
//  * of conditions and the following disclaimer.
//  *
//  * Redistributions in binary form must reproduce the above copyright notice, this
//  * list of conditions and the following disclaimer in the documentation and/or
//  * other materials provided with the distribution.
//  *
//  * Neither the name of FIRST nor the names of its contributors may be used to endorse or
//  * promote products derived from this software without specific prior written permission.
//  *
//  * NO EXPRESS OR IMPLIED LICENSES TO ANY PARTY'S PATENT RIGHTS ARE GRANTED BY THIS
//  * LICENSE. THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
//  * "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO,
//  * THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
//  * ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE
//  * FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
//  * DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
//  * SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
//  * CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
//  * OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
//  * OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
//  */
package org.firstinspires.ftc.teamcode;

import com.qualcomm.robotcore.eventloop.opmode.Disabled;
import com.qualcomm.robotcore.eventloop.opmode.LinearOpMode;
import com.qualcomm.robotcore.eventloop.opmode.TeleOp;
import com.qualcomm.robotcore.hardware.DcMotor;
import com.qualcomm.robotcore.hardware.Servo;

import com.qualcomm.robotcore.util.ElapsedTime;
import com.qualcomm.robotcore.util.Range;
@TeleOp(name="TestHXG", group="Linear OpMode")

public class Final extends LinearOpMode {

    // 驱动电机
    private DcMotor LeftFrontDrive;
    private DcMotor LeftBehindDrive;
    private DcMotor RightFrontDrive;
    private DcMotor RightBehindDrive;

    // 机械臂电机
    private DcMotor Arm1;
    private DcMotor Arm2;

    // 舵机
    private Servo Wrist1;
    private Servo Wrist2;
    private Servo Fly1;
     // 计时器
    private ElapsedTime runtime = new ElapsedTime();
    @Override
    public void runOpMode() {

        // 初始化驱动电机
        LeftFrontDrive = hardwareMap.get(DcMotor.class, "LeftFrontMotor");
        LeftBehindDrive = hardwareMap.get(DcMotor.class, "LeftBehindMotor");
        RightFrontDrive = hardwareMap.get(DcMotor.class, "RightFrontMotor");
        RightBehindDrive = hardwareMap.get(DcMotor.class, "RightBehindMotor");

        // 初始化机械臂电机
        Arm1 = hardwareMap.get(DcMotor.class, "Arm1");
        Arm2 = hardwareMap.get(DcMotor.class, "Arm2");

        // 初始化舵机
        Wrist1 = hardwareMap.get(Servo.class, "Wrist1");
        Wrist2 = hardwareMap.get(Servo.class, "Wrist2");
        Fly1 = hardwareMap.get(Servo.class, "Fly1");
        // 设置电机方向
        LeftFrontDrive.setDirection(DcMotor.Direction.REVERSE);
        LeftBehindDrive.setDirection(DcMotor.Direction.REVERSE);
        RightFrontDrive.setDirection(DcMotor.Direction.FORWARD);
        RightBehindDrive.setDirection(DcMotor.Direction.FORWARD);

        // 设置电机零功率行为
        LeftFrontDrive.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
        LeftBehindDrive.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
        RightFrontDrive.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
        RightBehindDrive.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
        Arm1.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
        Arm2.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);



        // 重置编码器位置并设置初始位置为0
        Arm1.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
        Arm2.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);

        // Wait for the game to start (driver presses PLAY)
        waitForStart();


        // run until the end of the match (driver presses STOP)
        while (opModeIsActive()) {
        //chassis
        double y = -gamepad1.left_stick_y;
        double x = gamepad1.left_stick_x;
        double turn = gamepad1.right_stick_x;
        double denominator = Math.max(Math.abs(y) + Math.abs(x) + Math.abs(turn), 1);

        if (Math.abs(y) < 0.1 && Math.abs(x) < 0.1 && Math.abs(turn) < 0.1) {
            // 停止所有驱动电机
            LeftFrontDrive.setPower(0.0);
            LeftBehindDrive.setPower(0.0);
            RightFrontDrive.setPower(0.0);
            RightBehindDrive.setPower(0.0);
        } else {
            // 控制驱动电机
            LeftFrontDrive.setPower((y + x + turn) / denominator);
            LeftBehindDrive.setPower((y - x + turn) / denominator);
            RightFrontDrive.setPower((y - x - turn) / denominator);
            RightBehindDrive.setPower((y + x - turn) / denominator);
        }
        // D-pad上键控制Wrist1顺时针旋转
        if (gamepad1.left_bumper) {
            Wrist1.setPosition(0.5);
        }// D-pad下键控制Wrist1逆时针旋转
        else if (gamepad1.right_bumper) {
            Wrist1.setPosition(0.7);
        }
        // D-pad左键控制Wrist2顺时针旋转
        if (gamepad1.left_trigger>=0.5) {
            Wrist2.setPosition(0.7);
        }     // D-pad右键控制Wrist2逆时针旋转
        else if (gamepad1.right_trigger>=0.5) {
            Wrist2.setPosition(0.8);
        }
            //大臂位置
            if(gamepad1.dpad_down){
                Arm1.setPower(0.5);
                Arm1.setTargetPosition(0);
                Arm1.setMode(DcMotor.RunMode.RUN_TO_POSITION);
            }
            else if(gamepad1.dpad_up){
                Arm1.setPower(0.8);
                Arm1.setTargetPosition(-1000);
                Arm1.setMode(DcMotor.RunMode.RUN_TO_POSITION);
            }
            else if(gamepad1.dpad_right){
                Arm1.setPower(0.8);
                Arm1.setTargetPosition(-500);
                Arm1.setMode(DcMotor.RunMode.RUN_TO_POSITION);
                
                // 机器人放像素时大臂的位置（这里在中间向外微偏）
            }

         //lift
            if(gamepad1.y){
                Arm2.setPower(0.8);
                Arm2.setTargetPosition(0);
                Arm2.setMode(DcMotor.RunMode.RUN_TO_POSITION);
            }
            else if(gamepad1.a){
                Arm2.setPower(0.8);
                Arm2.setTargetPosition(-1150);
                Arm2.setMode(DcMotor.RunMode.RUN_TO_POSITION);
            }
            else if(gamepad1.b){
                Arm2.setPower(0.8);
                Arm2.setTargetPosition(300);
                Arm2.setMode(DcMotor.RunMode.RUN_TO_POSITION);
                
                //机器人放像素时大臂的位置（这里在中间向外微偏）
            }
            
            //combine
            if (gamepad1.dpad_left && gamepad1.a) {
                // 触发 dpad_right 的功能
                Arm1.setPower(0.8);
                Arm1.setTargetPosition(-500);
                Arm1.setMode(DcMotor.RunMode.RUN_TO_POSITION);

                // 触发 A 键的功能
                Arm2.setPower(0.8);
                Arm2.setTargetPosition(-1150);
                Arm2.setMode(DcMotor.RunMode.RUN_TO_POSITION);

                
            }

            // dpad_left 和 B 一起按触发连锁动作
            if (gamepad1.dpad_left && gamepad1.b) {
                // 触发 Y 键的功能
                Arm2.setPower(0.8);
                Arm2.setTargetPosition(90);
                Arm2.setMode(DcMotor.RunMode.RUN_TO_POSITION);

                // 触发 dpad_down 的功能
                Arm1.setPower(0.5);
                Arm1.setTargetPosition(0);
                Arm1.setMode(DcMotor.RunMode.RUN_TO_POSITION);

                // 触发 left_trigger 的功能
                Wrist2.setPosition(0.6);
            }
            if (gamepad1.dpad_left && gamepad1.y) {
                Fly1.setPosition(5.0); // 假设 0.0 是逆时针旋转的目标位置
            }
            
            // dpad_left 和 left_trigger 一起按触发机器人向左平移3秒
            if (gamepad1.dpad_left && gamepad1.left_trigger >= 0.5) {
                runtime.reset();
                while (runtime.seconds() < 3.0 && opModeIsActive()) {
                    LeftFrontDrive.setPower(-0.5);
                    LeftBehindDrive.setPower(0.5);
                    RightFrontDrive.setPower(0.5);
                    RightBehindDrive.setPower(-0.5);
                }
                // 停止所有驱动电机
                LeftFrontDrive.setPower(0.0);
                LeftBehindDrive.setPower(0.0);
                RightFrontDrive.setPower(0.0);
                RightBehindDrive.setPower(0.0);
                Wrist2.setPosition(0.7);

            }
                   // dpad_left 和 right_trigger 一起按触发机器人向右平移3秒（AUTO MOOD）
            if (gamepad1.dpad_left && gamepad1.right_trigger >= 0.5) {
                runtime.reset();
                while (runtime.seconds() < 3.0 && opModeIsActive()) {
                    LeftFrontDrive.setPower(0.5);
                    LeftBehindDrive.setPower(-0.5);
                    RightFrontDrive.setPower(-0.5);
                    RightBehindDrive.setPower(0.5);
                }
                // 停止所有驱动电机
                LeftFrontDrive.setPower(0.0);
                LeftBehindDrive.setPower(0.0);
                RightFrontDrive.setPower(0.0);
                RightBehindDrive.setPower(0.0);
                Wrist2.setPosition(0.7);

            }

# TechBY DreamerX1 人形机器人开源
 
      Arduino IDE(程序仅供参考)
      #include <ZhouyuMAX.h>
      if(ZhouyuMAX.readSonicSensorCM(A1)< 40){
      ZhouyuMAX.setMotorSpeeds(0,0);//停止前进
      ZhouyuMAX ZhouyuMAX;
      void setup(){
      ZhouyuMAX.ZhouyuBegin();
      ZhouyuMAX.setMotorSpeeds(200,(-200));//驱动电机
      ZhouyuMAX.setServoPosition(1,10);//左肩舵机
      ZhouyuMAX.setServoPosition(2,170);右肩舵机
      ZhouyuMAX.setServoPosition(3,90);//左肘舵机
      ZhouyuMAX.setServoPosition(4,90);//右肘舵机
      ZhouyuMAX.setServoPosition(5,90);//头部舵机
      ZhouyuMAX.setServoPosition(6,150);//右夹持器
      delay(1000);
      ZhouyuMAX.setServoPosition(3,30);//左臂向后
      ZhouyuMAX.setServoPosition(4,10);//右臂向前
      delay(2000);
      ZhouyuMAX.setServoPosition(6,20);//右夹持器松开
      delay(1000);
      ZhouyuMAX.setServoPosition(1,10);
      ZhouyuMAX.setServoPosition(2,170)
      ZhouyuMAX.setServoPosition(3,90);
      ZhouyuMAX.setServoPosition(4,90);
      ZhouyuMMAX.setServoPosition(5,90);
      void loop({
      ZhouyuMAX.setMotorSpeeds(200,(-200));
      if (ZhouyuMAX.readSonicSensorCM(A2)<40){
      ZhouyuMAX.setMotorSpeeds(0,0);
      ZhouyuMAX.setServoPosition(4,10);
      delay(2000);
      ZhouyuMAX.setServoPosition(6,20);
      delay(2000);
      ZhouyuMAX.setServoPosition(6,150);
      delay(2000);
      ZhouyuMAX.setServoPosition(4,50);//右臂向前，右夹持器张开闭合。(模拟拿花的过程)
      ZhouyuMAX.setMotorSpeeds(200,(-200));
      ZhouyuMAX,setMotorSpeeds(200,(-200));
      } else {
      ZhouyuMAX.setServoPosition(6,150);//恢复初始状态
      delay(1000);
      ZhouyuMAX.setServoPosition(1,130);
      ZhouyuMAX.setServoPosition(2,50);//左肩、右肩同时张开
      ZhouyuMAX,setServoPosition(3.170):
      ZhouyuMAX,setServoPosition(4,10);//左臂、右臂同时向前(模拟拥抱状态)
      delay(4000);
      ZhouyuMAX,setServoPosition(3,90);
      ZhouyuMAX.setServoPosition(4,.90)
      ZhouyuMAX.setServoPosition(1,10);
      ZhouyuMAX.setServoPosition(2,170);//恢复初始状态
      delay(3000);
      ZhouyuMAX.setServoPosition(1,130);
      ZhouyuMAX.setServoPosition(2,80);
      ZhouyuMAX.setServoPosition(3,170):
      ZhouyuMAX.setServoPosition(5,150);//向右转头的同时，右臂抬起，左臂弯曲向前抬起(模拟高兴的状态)
      delay(3000);
      ZhouyuMAX.setServoPosition(1,10);
      ZhouyuMAX.setServoPosition(2,170);
      ZhouyuMAX.setServoPosition(3,90);
      ZhouyuMAX.setServoPosition(4.90)
      ZhouyuMAX.setServoPosition(5,90);
      delay(3000);
      ZhouyuMAX.setServoPosition(1,100);
      ZhouyuMAX.setServoPosition(2,50);
      ZhouyuMAX.setServoPosition(4,10);
      ZhouyuMAX.setServoPosition(5,30);//向左转头的同时，左臂抬起，右臂弯曲向前抬起delay(3000);
      } else {
      ZhouyuMAX.setServoPosition(1,10);ZhouyuMAX.setServoPosition(2,170);
      ZhouyuMAX.setServoPosition(3,90);
      ZhouyuMAX.setServoPosition(4,90);
      ZhouyuMAX.setServoPosition(5,90);
      ZhouyuMAX.setServoPosition(6,150)
     
            }
        }
    }
