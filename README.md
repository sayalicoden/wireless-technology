# wireless-technology
clc;
clear;
close all;

%% Parameters
N = 1e6;                % Number of transmitted bits
snr_db = 0:2:12;        % SNR range in dB
ber = zeros(size(snr_db));

%% Generate Random Bits
bits = randi([0 1],1,N);

%% BPSK Modulation
tx = 2*bits - 1;        % 0 -> -1 , 1 -> +1

%% Simulation Loop
for i = 1:length(snr_db)

    snr = 10^(snr_db(i)/10);      % Convert dB to linear

    noise = randn(1,N)/sqrt(2*snr); % AWGN noise

    rx = tx + noise;               % Received signal

    detected = rx > 0;             % BPSK detection

    ber(i) = sum(bits ~= detected)/N;  % BER calculation

end

%% Plot BER vs SNR
semilogy(snr_db,ber,'o-','LineWidth',2)
grid on
xlabel('SNR (dB)')
ylabel('Bit Error Rate (BER)')
title('BER vs SNR for BPSK over AWGN Channel')
