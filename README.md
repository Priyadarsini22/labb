PROGRAM1:

CODE:

set ns [new Simulator]

set tf [open p1.tr w]
$ns trace-all $tf

set nf [open p1.nam w]
$ns namtrace-all $nf

set n0 [$ns node]
set n1 [$ns node]
set n2 [$ns node]
set n3 [$ns node]

$ns duplex-link $n0 $n2 2Mb 2ms DropTail
$ns duplex-link $n1 $n2 2Mb 2ms DropTail
$ns duplex-link $n2 $n3 0.4Mb 10ms DropTail
$ns queue-limit $n2 $n3 5

set udp1 [new Agent/UDP]
$ns attach-agent $n0 $udp1
set null1 [new Agent/Null]
$ns attach-agent $n3 $null1
$ns connect $udp1 $null1
set cbr1 [new Application/Traffic/CBR]
$cbr1 attach-agent $udp1
$ns at 1.1 "$cbr1 start"
set tcp1 [new Agent/TCP]
$ns attach-agent $n1 $tcp1
set sink1 [new Agent/TCPSink]
$ns attach-agent $n3 $sink1
$ns connect $tcp1 $sink1
set ftp1 [new Application/FTP]
$ftp1 attach-agent $tcp1
$ns at 0.1 "$ftp1 start"
$ns at 10.0 "finish"
proc finish {} {
global ns tf nf
$ns flush-trace
close $tf
close $nf
puts "running nam..."
exec nam p1.nam &
exit 0
}
$ns run

BEGIN{
tcp_count=0;
udp_count=0;
}
{
if ($1 == "d" && $5 == "tcp")
	tcp_count++;
if ($1 == "d" && $5 =="cbr")
	udp_count++;
}
END{
printf("tcp-count=%d\n",tcp_count);
printf("udp_count=%d\n",udp_count);
	
}


#2nd pg
set ns [new Simulator]
set nf [open pro2.nam w]
set tf [open pro2.tr w]
set cwind [open win2.tr w]
$ns color 1 Blue
$ns color 2 Red
$ns namtrace-all $nf
$ns trace-all $tf
set n0 [$ns node]
set n1 [$ns node]
set n2 [$ns node]
set n3 [$ns node]
set n4 [$ns node]
set n5 [$ns node]
$ns duplex-link $n0 $n2 2Mb 2ms DropTail
$ns duplex-link $n1 $n2 2Mb 2ms DropTail
$ns duplex-link $n2 $n3 0.4Mb 5ms DropTail
$ns duplex-link $n3 $n4 2Mb 2ms DropTail
$ns duplex-link $n3 $n5 2Mb 2ms DropTail
$ns queue-limit $n2 $n3 10
set tcp1 [new Agent/TCP]
set sink1 [new Agent/TCPSink]
set ftp1 [new Application/FTP]
$ns attach-agent $n0 $tcp1
$ns attach-agent $n5 $sink1
$ns connect $tcp1 $sink1
$ftp1 attach-agent $tcp1
$ns at 1.2 "$ftp1 start"
set tcp2 [new Agent/TCP]
set sink2 [new Agent/TCPSink]
set telnet1 [new Application/Telnet]
$ns attach-agent $n1 $tcp2
$ns attach-agent $n4 $sink2
$ns connect $tcp2 $sink2
$telnet1 attach-agent $tcp2

$ns at 5.1 "$telnet1 start"

$ns at 5.0 "$ftp1 stop"

$ns at 10.0 "finish"

proc plotWindow {tcpSource file} {

global ns

set time 0.01

set now [$ns now]
set cwind [$tcpSource set cwnd_]
puts $file "$now $cwind"
$ns at [expr $now+$time] "plotWindow $tcpSource $file"
}
$ns at 2.0 "plotWindow $tcp1 $cwind"
$ns at 5.5 "plotWindow $tcp2 $cwind"
proc finish {} {
global ns tf nf cwind
$ns flush-trace
close $tf
close $nf
puts "running..."
exec nam pro2.nam &
exec xgraph win2.tr &
exit 0
}


p3
set ns [new Simulator]

set tf [open p2.tr w]
$ns trace-all $tf

set nf [open p2.nam w]
$ns namtrace-all $nf

set cwnd [open win3.tr w]

$ns color 1 Blue
$ns color 2 Red
$ns rtproto DV

set n0 [$ns node]
set n1 [$ns node]
set n2 [$ns node]
set n3 [$ns node]
set n4 [$ns node]
set n5 [$ns node]
$ns duplex-link $n0 $n1 0.3Mb 10ms DropTail
$ns duplex-link $n0 $n2 0.3Mb 10ms DropTail
$ns duplex-link $n1 $n4 0.3Mb 10ms DropTail
$ns duplex-link $n2 $n3 0.3Mb 10ms DropTail
$ns duplex-link $n3 $n5 0.3Mb 10ms DropTail
$ns duplex-link $n4 $n5 0.3Mb 10ms DropTail

$ns duplex-link-op $n0 $n1 orient right-up
$ns duplex-link-op $n0 $n2 orient right-down
$ns duplex-link-op $n2 $n3 orient right
$ns duplex-link-op $n1 $n4 orient right
$ns duplex-link-op $n3 $n5 orient right-up
$ns duplex-link-op $n4 $n5 orient right-down

set tcp [new Agent/TCP]
$ns attach-agent $n0 $tcp

set sink [new Agent/TCPSink]
$ns attach-agent $n5 $sink

$ns connect $tcp $sink
$tcp set fid_ 1

set ftp [new Application/FTP]
$ftp attach-agent $tcp

$ns rtmodel-at 1.0 down $n1 $n4
$ns rtmodel-at 3.0 up $n1 $n4


$ns at 0.1 "$ftp start"
$ns at 12.0 "finish"

proc plotWindow {tcpSource file} {
     global ns
     set time 0.01
     set now [$ns now]
     set cwnd [$tcpSource set cwnd_]
     puts $file "$now $cwnd"
     $ns at [expr $now+$time] "plotWindow $tcpSource $file"
}

$ns at 1.0 "plotWindow $tcp $cwnd"

proc finish {} {
     global ns tf nf cwnd
     $ns flush-trace
     close $tf
     close $nf
     exec nam p2.nam &
     exec xgraph win3.tr &
     exit 0
}






PROGRAM 4

set ns [new Simulator]

set tf [open program4.tr w]
$ns trace-all $tf

set nf [open program4.nam w]
$ns namtrace-all $nf

set s [$ns node]
set c [$ns node]

$ns color 1 Blue

$s label "Server"
$c label "Client"

$ns duplex-link $s $c 10Mb 22ms DropTail
$ns duplex-link-op $s $c orient right

set tcp0 [new Agent/TCP]
$ns attach-agent $s $tcp0
$tcp0 set packetSize_ 1500

set sink0 [new Agent/TCPSink]
$ns attach-agent $c $sink0

$ns connect $tcp0 $sink0

set ftp0 [new Application/FTP]
$ftp0 attach-agent $tcp0

$tcp0 set fid_ 1

proc finish {} {
	global ns tf nf
	$ns flush-trace
	close $tf
	close $nf
	exec nam program4.nam &
	exec awk -f ex5transfer.awk program4.tr &
	exec awk -f ex5convert.awk program4.tr>convert.tr &
	exec xgraph convert.tr &
	}
$ns at 0.01 "$ftp0 start"
$ns at 15.0 "$ftp0 stop"
$ns at 15.1 "finish"
$ns run
BEGIN{
	count = 0;
	time = 0;
	total_bytes_sent = 0;
	total_bytes_received = 0;
	}
{
	if ($1 == "r" && $4 == 1 && $5 == "tcp")
		total_bytes_received += $6;
	if ($1 == "+" && $3 == 0 && $5 == "tcp")
		total_bytes_sent += $6;
}
END{
	system("clear");
	printf("\n Transmission time required to transfer file is %f", $2);
	printf("\n Actual data sent from the server is %f Mbps", (total_bytes_sent)/1000000);
	printf("\n Data received by the client is %f Mbps \n", (total_bytes_received)/1000000);
}
	
BEGIN{
	count = 0;
	time = 0;
	}
{
	if($1 == "r" && $4 == 1 && $5 == "tcp")
	{
		count += $6;
		time = $2;
		printf("\n %f \t %f", time, (count)/1000000);
	}
}
END{
}






PROGRAM 5

set ns [new Simulator -multicast on]

set tf [open mcast.tr w]
$ns trace-all $tf

set fd [open mcast.nam w]
$ns namtrace-all $fd

set n0 [$ns node]
set n1 [$ns node]
set n2 [$ns node]
set n3 [$ns node]
set n4 [$ns node]
set n5 [$ns node]
set n6 [$ns node]
set n7 [$ns node]

$ns duplex-link $n0 $n2 1.5Mb 10ms DropTail
$ns duplex-link $n1 $n2 1.5Mb 10ms DropTail
$ns duplex-link $n2 $n3 1.5Mb 10ms DropTail
$ns duplex-link $n3 $n4 1.5Mb 10ms DropTail
$ns duplex-link $n3 $n7 1.5Mb 10ms DropTail
$ns duplex-link $n4 $n5 1.5Mb 10ms DropTail
$ns duplex-link $n4 $n6 1.5Mb 10ms DropTail

set mproto DM
set mrthandle [$ns mrtproto $mproto {}]

set group1 [Node allocaddr]
set group2 [Node allocaddr]

set udp0 [new Agent/UDP]
$ns attach-agent $n0 $udp0
$udp0 set dst_addr_ $group1
$udp0 set dst_port_ 0
set cbr1 [new Application/Traffic/CBR]
$cbr1 attach-agent $udp0

set udp1 [new Agent/UDP]
$ns attach-agent $n1 $udp1
$udp1 set dst_addr_ $group2
$udp1 set dst_port_ 0
set cbr2 [new Application/Traffic/CBR]
$cbr2 attach-agent $udp1

set rvcr1 [new Agent/Null]
$ns attach-agent $n5 $rvcr1
$ns at 1.0 "$n5 join-group $rvcr1 $group1"

set rvcr2 [new Agent/Null]
$ns attach-agent $n6 $rvcr2
$ns at 1.5 "$n6 join-group $rvcr2 $group1"

set rvcr3 [new Agent/Null]
$ns attach-agent $n7 $rvcr3
$ns at 2.0 "$n7 join-group $rvcr3 $group1"

set rvcr4 [new Agent/Null]
$ns attach-agent $n5 $rvcr1
$ns at 2.5 "$n5 join-group $rvcr4 $group2"

set rvcr5 [new Agent/Null]
$ns attach-agent $n6 $rvcr2
$ns at 3.0 "$n6 join-group $rvcr5 $group2"

set rvcr6 [new Agent/Null]
$ns attach-agent $n7 $rvcr3
$ns at 3.5 "$n7 join-group $rvcr6 $group2"

$ns at 4.0 "$n5 leave-group $rvcr1 $group1"
$ns at 4.5 "$n6 leave-group $rvcr2 $group1"
$ns at 5.0 "$n7 leave-group $rvcr3 $group1"

$ns at 5.5 "$n5 leave-group $rvcr4 $group2"
$ns at 6.0 "$n6 leave-group $rvcr5 $group2"
$ns at 6.5 "$n7 leave-group $rvcr6 $group2"

$ns at 0.5 "$cbr1 start"
$ns at 9.5 "$cbr1 stop"

$ns at 0.5 "$cbr2 start"
$ns at 9.5 "$cbr2 stop"

$ns at 10.0 "finish"

$n0 label "Source 1"

$n1 label "Source 2"

$ns color 1 pink
$ns color 2 purple

$n5 label "Receiver 1"
$n5 color violet

$n6 label "Receiver 2"
$n6 color violet

$n7 label "Receiver 3"
$n7 color violet

$udp0 set fid_ 1
$udp1 set fid_ 2


proc finish {} {
global ns tf fd
$ns flush-trace
close $tf
close $fd
puts "running nam..."
exec nam mcast.nam &
exit 0
}

$ns run
