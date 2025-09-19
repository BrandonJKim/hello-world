package org.firstinspires.ftc.teamcode;

import com.qualcomm.robotcore.eventloop.opmode.Autonomous;
import com.qualcomm.robotcore.eventloop.opmode.OpMode;
import com.qualcomm.robotcore.eventloop.opmode.TeleOp;
import com.qualcomm.robotcore.hardware.DcMotor;
import com.qualcomm.robotcore.hardware.DcMotorSimple;
import com.qualcomm.robotcore.util.ElapsedTime;

@TeleOp(name = "DecodeRobotTeleOp", group = "TeleOp")
public class DecodeRobotTeleOp extends OpMode {

    // Drive motors
    private DcMotor leftDrive;
    private DcMotor rightDrive;

    // Intake motor (rotating ground intake)
    private DcMotor intakeMotor;

    // Flywheel motors (2x 6000 rpm motors)
    private DcMotor flywheelLeft;
    private DcMotor flywheelRight;

    @Override
    public void init() {
        leftDrive = hardwareMap.get(DcMotor.class, "left_drive");
        rightDrive = hardwareMap.get(DcMotor.class, "right_drive");
        intakeMotor = hardwareMap.get(DcMotor.class, "intake_motor");
        flywheelLeft = hardwareMap.get(DcMotor.class, "flywheel_left");
        flywheelRight = hardwareMap.get(DcMotor.class, "flywheel_right");

        // Set directions
        leftDrive.setDirection(DcMotor.Direction.FORWARD);
        rightDrive.setDirection(DcMotor.Direction.REVERSE); // Usually reversed

        intakeMotor.setDirection(DcMotor.Direction.FORWARD);

        flywheelLeft.setDirection(DcMotor.Direction.FORWARD);
        flywheelRight.setDirection(DcMotor.Direction.REVERSE); // To spin both flywheels same direction

        // Set zero power behavior
        leftDrive.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
        rightDrive.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
        intakeMotor.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
        flywheelLeft.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
        flywheelRight.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
    }

    @Override
    public void loop() {
        // Driving controls - tank drive
        double leftPower = -gamepad1.left_stick_y;
        double rightPower = -gamepad1.right_stick_y;

        leftDrive.setPower(leftPower);
        rightDrive.setPower(rightPower);

        // Intake control - gamepad2 right trigger pulls in, left trigger reverses intake
        double intakePower = gamepad2.right_trigger - gamepad2.left_trigger;
        intakeMotor.setPower(intakePower);

        // Flywheel control - toggle with gamepad2 'a' button
        // Hold 'a' to shoot, release to stop
        if (gamepad2.a) {
            flywheelLeft.setPower(1.0);  // Full power (6000 RPM motors)
            flywheelRight.setPower(1.0);
        } else {
            flywheelLeft.setPower(0);
            flywheelRight.setPower(0);
        }

        telemetry.addData("Left Drive Power", leftPower);
        telemetry.addData("Right Drive Power", rightPower);
        telemetry.addData("Intake Power", intakePower);
        telemetry.addData("Flywheel Running", gamepad2.a);
        telemetry.update();
    }
}
package org.firstinspires.ftc.teamcode;

import com.qualcomm.robotcore.eventloop.opmode.Autonomous;
import com.qualcomm.robotcore.eventloop.opmode.LinearOpMode;
import com.qualcomm.robotcore.hardware.DcMotor;

@Autonomous(name = "DecodeRobotAuto", group = "Autonomous")
public class DecodeRobotAuto extends LinearOpMode {

    private DcMotor leftDrive;
    private DcMotor rightDrive;
    private DcMotor flywheelLeft;
    private DcMotor flywheelRight;

    @Override
    public void runOpMode() throws InterruptedException {
        leftDrive = hardwareMap.get(DcMotor.class, "left_drive");
        rightDrive = hardwareMap.get(DcMotor.class, "right_drive");
        flywheelLeft = hardwareMap.get(DcMotor.class, "flywheel_left");
        flywheelRight = hardwareMap.get(DcMotor.class, "flywheel_right");

        leftDrive.setDirection(DcMotor.Direction.FORWARD);
        rightDrive.setDirection(DcMotor.Direction.REVERSE);

        flywheelLeft.setDirection(DcMotor.Direction.FORWARD);
        flywheelRight.setDirection(DcMotor.Direction.REVERSE);

        waitForStart();

        // Drive forward for 2 seconds
        leftDrive.setPower(0.5);
        rightDrive.setPower(0.5);
        sleep(2000);

        // Stop driving
        leftDrive.setPower(0);
        rightDrive.setPower(0);

        // Spin up flywheels
        flywheelLeft.setPower(1.0);
        flywheelRight.setPower(1.0);

        // Shoot for 2 seconds
        sleep(2000);

        // Stop flywheels
        flywheelLeft.setPower(0);
        flywheelRight.setPower(0);
    }
}

